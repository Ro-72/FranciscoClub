<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'
import * as THREE from 'three'

const canvas = ref(null)
const wrapper = ref(null)
let animationFrame = 0
let renderer
let scene
let camera
let courtGroup
let ball
let resizeObserver
let intersectionObserver
let reducedMotion = false
let isVisible = true
let kickPulse = 0
const players = []
const pointer = { x: 0, y: 0 }
const targetRotation = { x: -0.07, y: 0 }

function line(points, material) {
  const geometry = new THREE.BufferGeometry().setFromPoints(points)
  const mesh = new THREE.Line(geometry, material)
  courtGroup.add(mesh)
  return mesh
}

function rectangle(width, height, y, material) {
  const halfWidth = width / 2
  const halfHeight = height / 2
  return line(
    [
      new THREE.Vector3(-halfWidth, y, -halfHeight),
      new THREE.Vector3(halfWidth, y, -halfHeight),
      new THREE.Vector3(halfWidth, y, halfHeight),
      new THREE.Vector3(-halfWidth, y, halfHeight),
      new THREE.Vector3(-halfWidth, y, -halfHeight),
    ],
    material,
  )
}

function circle(radius, y, material) {
  const points = []
  for (let index = 0; index <= 64; index += 1) {
    const angle = (index / 64) * Math.PI * 2
    points.push(new THREE.Vector3(Math.cos(angle) * radius, y, Math.sin(angle) * radius))
  }
  return line(points, material)
}

function addGoal(x, material) {
  const group = new THREE.Group()
  const post = new THREE.Mesh(new THREE.CylinderGeometry(0.025, 0.025, 0.8, 8), material)
  post.position.set(x, 0.4, 0)
  group.add(post)
  const crossbar = new THREE.Mesh(new THREE.CylinderGeometry(0.025, 0.025, 1.8, 8), material)
  crossbar.rotation.z = Math.PI / 2
  crossbar.position.set(x + (x > 0 ? -0.9 : 0.9), 0.8, 0)
  group.add(crossbar)
  const net = new THREE.Mesh(
    new THREE.BoxGeometry(0.72, 0.62, 1.8),
    new THREE.MeshBasicMaterial({ color: 0xc99a3b, transparent: true, opacity: 0.08, wireframe: true }),
  )
  net.position.set(x + (x > 0 ? -0.37 : 0.37), 0.42, 0)
  group.add(net)
  courtGroup.add(group)
}

function addPlayer(x, z, material, phase) {
  const player = new THREE.Group()
  const body = new THREE.Mesh(new THREE.CylinderGeometry(0.11, 0.14, 0.34, 8), material)
  body.position.y = 0.43
  player.add(body)
  const head = new THREE.Mesh(new THREE.SphereGeometry(0.11, 12, 8), material)
  head.position.y = 0.68
  player.add(head)
  const foot = new THREE.Mesh(new THREE.BoxGeometry(0.25, 0.06, 0.14), material)
  foot.position.set(0.03, 0.25, 0)
  player.add(foot)
  player.position.set(x, 0, z)
  courtGroup.add(player)
  players.push({ group: player, baseZ: z, phase })
}

function addRod(x, material) {
  const rod = new THREE.Mesh(new THREE.CylinderGeometry(0.025, 0.025, 4.9, 8), material)
  rod.rotation.x = Math.PI / 2
  rod.position.set(x, 0.75, 0)
  courtGroup.add(rod)
}

function buildCourt() {
  const gold = new THREE.LineBasicMaterial({ color: 0xe7c06a, transparent: true, opacity: 0.95 })
  const dimGold = new THREE.LineBasicMaterial({ color: 0xc99a3b, transparent: true, opacity: 0.45 })
  const teamBlue = new THREE.MeshStandardMaterial({ color: 0x88a8af, roughness: 0.35, metalness: 0.2 })
  const teamGold = new THREE.MeshStandardMaterial({ color: 0xe7c06a, roughness: 0.35, metalness: 0.2 })
  const floor = new THREE.Mesh(
    new THREE.BoxGeometry(9.2, 0.16, 5.2),
    new THREE.MeshStandardMaterial({ color: 0x1a2521, roughness: 0.8, metalness: 0.08 }),
  )
  floor.position.y = -0.12
  courtGroup.add(floor)

  const pitch = new THREE.Mesh(new THREE.PlaneGeometry(8.7, 4.7), new THREE.MeshStandardMaterial({ color: 0x274b3a, roughness: 0.9 }))
  pitch.rotation.x = -Math.PI / 2
  courtGroup.add(pitch)
  rectangle(8.5, 4.5, 0.02, gold)
  line([new THREE.Vector3(0, 0.025, -2.25), new THREE.Vector3(0, 0.025, 2.25)], gold)
  circle(0.62, 0.025, gold)
  circle(0.08, 0.03, gold)
  const leftArea = rectangle(1.4, 2.2, 0.025, dimGold)
  leftArea.position.x = -3.55
  const rightArea = rectangle(1.4, 2.2, 0.025, dimGold)
  rightArea.position.x = 3.55

  for (let index = -3; index <= 3; index += 1) {
    const stripe = new THREE.Mesh(new THREE.PlaneGeometry(0.56, 4.45), new THREE.MeshBasicMaterial({ color: index % 2 === 0 ? 0x315842 : 0x274b3a, transparent: true, opacity: 0.6 }))
    stripe.rotation.x = -Math.PI / 2
    stripe.position.set(index * 1.08, 0.012, 0)
    courtGroup.add(stripe)
  }
  addGoal(-4.3, gold)
  addGoal(4.3, gold)

  const rodMaterial = new THREE.MeshStandardMaterial({ color: 0xc99a3b, metalness: 0.8, roughness: 0.3 })
  ;[-2.55, -0.85, 0.85, 2.55].forEach((x, index) => {
    addRod(x, rodMaterial)
    addPlayer(x, index % 2 === 0 ? -1.3 : 1.3, index % 2 === 0 ? teamBlue : teamGold, index * 0.9)
    addPlayer(x, index % 2 === 0 ? 0 : -0.1, index % 2 === 0 ? teamBlue : teamGold, index * 0.9 + 1)
    addPlayer(x, index % 2 === 0 ? 1.3 : -1.3, index % 2 === 0 ? teamBlue : teamGold, index * 0.9 + 2)
  })

  const light = new THREE.PointLight(0xe7c06a, 1.8, 5)
  light.position.set(0, 4, 1.5)
  courtGroup.add(light)
  const texture = new THREE.TextureLoader().load(`${import.meta.env.BASE_URL}escudo-franciscos-club.png`)
  const sign = new THREE.Mesh(new THREE.PlaneGeometry(1, 1), new THREE.MeshBasicMaterial({ map: texture, transparent: true }))
  sign.position.set(0, 1.5, -2.16)
  courtGroup.add(sign)

  ball = new THREE.Mesh(new THREE.SphereGeometry(0.15, 20, 14), new THREE.MeshStandardMaterial({ color: 0xf3efe5, roughness: 0.28, metalness: 0.1 }))
  ball.position.set(0, 0.17, 0)
  courtGroup.add(ball)
}

function handlePointerMove(event) {
  const bounds = wrapper.value.getBoundingClientRect()
  pointer.x = ((event.clientX - bounds.left) / bounds.width - 0.5) * 2
  pointer.y = ((event.clientY - bounds.top) / bounds.height - 0.5) * 2
  targetRotation.y = pointer.x * 0.16
  targetRotation.x = -0.07 + pointer.y * 0.05
}

function handlePointerLeave() {
  pointer.x = 0
  pointer.y = 0
  targetRotation.x = -0.07
  targetRotation.y = 0
}

function handleCourtTap() {
  kickPulse = 1
}

function handleVisibility(entries) {
  isVisible = entries[0]?.isIntersecting ?? true
  if (!isVisible && animationFrame) {
    cancelAnimationFrame(animationFrame)
    animationFrame = 0
  }
  if (isVisible && !reducedMotion && !animationFrame) {
    animationFrame = requestAnimationFrame(render)
  }
}

function render(time = 0) {
  if (!renderer || !isVisible) return
  courtGroup.rotation.x += (targetRotation.x - courtGroup.rotation.x) * 0.06
  courtGroup.rotation.y += (targetRotation.y - courtGroup.rotation.y) * 0.06
  if (!reducedMotion) {
    const speed = kickPulse > 0 ? 0.0018 : 0.001
    ball.position.x = Math.sin(time * speed) * 3.25
    ball.position.z = Math.cos(time * speed * 1.45) * 1.45
    ball.position.y = 0.18 + Math.abs(Math.sin(time * speed * 2.2)) * 0.18
    ball.rotation.x += 0.08
    ball.rotation.z += 0.1
    players.forEach(({ group, baseZ, phase }) => {
      group.position.z = baseZ + Math.sin(time * 0.0016 + phase) * 0.1
      group.rotation.y = Math.sin(time * 0.002 + phase) * 0.14
    })
    kickPulse = Math.max(kickPulse - 0.035, 0)
  }
  renderer.render(scene, camera)
  if (!reducedMotion) animationFrame = requestAnimationFrame(render)
}

function resize() {
  if (!canvas.value || !renderer || !camera) return
  const { clientWidth, clientHeight } = canvas.value.parentElement
  renderer.setSize(clientWidth, clientHeight, false)
  camera.aspect = clientWidth / clientHeight
  camera.updateProjectionMatrix()
}

onMounted(() => {
  reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  scene = new THREE.Scene()
  camera = new THREE.PerspectiveCamera(38, 1, 0.1, 100)
  camera.position.set(6.8, 5.5, 7.6)
  camera.lookAt(0, 0.35, 0)
  renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true, canvas: canvas.value, powerPreference: 'high-performance' })
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 1.7))
  renderer.outputColorSpace = THREE.SRGBColorSpace
  courtGroup = new THREE.Group()
  scene.add(courtGroup)
  scene.add(new THREE.HemisphereLight(0xf3efe5, 0x101112, 2.2))
  const keyLight = new THREE.DirectionalLight(0xe7c06a, 3.4)
  keyLight.position.set(-3, 6, 4)
  scene.add(keyLight)
  buildCourt()
  wrapper.value.addEventListener('pointermove', handlePointerMove)
  wrapper.value.addEventListener('pointerleave', handlePointerLeave)
  wrapper.value.addEventListener('click', handleCourtTap)
  resizeObserver = new ResizeObserver(resize)
  resizeObserver.observe(canvas.value.parentElement)
  intersectionObserver = new IntersectionObserver(handleVisibility, { threshold: 0.15 })
  intersectionObserver.observe(wrapper.value)
  resize()
  render()
})

onBeforeUnmount(() => {
  cancelAnimationFrame(animationFrame)
  wrapper.value?.removeEventListener('pointermove', handlePointerMove)
  wrapper.value?.removeEventListener('pointerleave', handlePointerLeave)
  wrapper.value?.removeEventListener('click', handleCourtTap)
  resizeObserver?.disconnect()
  intersectionObserver?.disconnect()
  renderer?.dispose()
})
</script>

<template>
  <div ref="wrapper" class="three-court three-court--story" role="img" aria-label="Cancha 3D interactiva con personajes jugando para Francisco’s Club">
    <canvas ref="canvas"></canvas>
    <div class="three-court-label"><span>Francisco’s Club</span><span>Cancha 01 · Juego libre</span></div>
    <span class="three-court-coordinate coordinate-one">mueve el cursor</span>
    <span class="three-court-coordinate coordinate-two">toca para patear</span>
  </div>
</template>
