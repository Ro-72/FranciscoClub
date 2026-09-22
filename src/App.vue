<script setup>
import { defineAsyncComponent, onBeforeUnmount, onMounted, ref } from 'vue'
import { club, moments, offerings } from './content/club'
import ChatbotWidget from './components/ChatbotWidget.vue'
import FloatingSocialBar from './components/FloatingSocialBar.vue'
import SocialCommunity from './components/SocialCommunity.vue'

const ThreeCourt = defineAsyncComponent(() => import('./components/ThreeCourt.vue'))

const hero = ref(null)
const shield = ref(null)
let scrollAnimationFrame = 0
let previousProgress = -1
let heroIsVisible = true
let heroObserver

function updateShield() {
  if (!heroIsVisible || scrollAnimationFrame) return
  if (window.scrollY >= 820 && previousProgress === 1) return

  scrollAnimationFrame = requestAnimationFrame(() => {
    const progress = Math.min(Math.max(window.scrollY / 820, 0), 1)

    if (progress !== previousProgress && shield.value) {
      shield.value.style.transform = `translate3d(0, ${progress * -42}px, 0) rotate(${progress * 7 - 3.5}deg)`
      previousProgress = progress
    }

    scrollAnimationFrame = 0
  })
}

function scrollToSection(id) {
  document.querySelector(id)?.scrollIntoView({ behavior: 'smooth' })
}

onMounted(() => {
  heroObserver = new IntersectionObserver(
    ([entry]) => {
      heroIsVisible = entry?.isIntersecting ?? true
      if (heroIsVisible) updateShield()
    },
    { rootMargin: '120px 0px' },
  )
  if (hero.value) heroObserver.observe(hero.value)
  updateShield()
  window.addEventListener('scroll', updateShield, { passive: true })
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', updateShield)
  cancelAnimationFrame(scrollAnimationFrame)
  heroObserver?.disconnect()
})
</script>

<template>
  <div class="site-shell">
    <FloatingSocialBar />
    <ChatbotWidget />

    <header class="site-header">
      <a class="brand" href="#inicio" aria-label="Francisco’s Club, volver al inicio">
        <span class="brand-mark"><img src="/escudo-franciscos-club.png" alt="" /></span>
        <span class="brand-copy"><strong>FRANCISCO’S</strong><small>CLUB</small></span>
      </a>
      <nav class="main-nav" aria-label="Navegación principal">
        <a href="#cancha">La cancha</a><a href="#momentos">La experiencia</a><a href="#oferta">El club</a><a href="#comunidad">Comunidad</a>
      </nav>
      <button class="header-action" type="button" @click="scrollToSection('#cancha')">Ver la cancha <span aria-hidden="true">↘</span></button>
    </header>

    <main>
      <section id="inicio" ref="hero" class="hero" aria-labelledby="hero-title">
        <div class="grain" aria-hidden="true"></div>
        <div class="hero-ambient ambient-left" aria-hidden="true"></div><div class="hero-ambient ambient-right" aria-hidden="true"></div>
        <div class="hero-content">
          <p class="eyebrow"><span class="live-dot"></span> Paucarpata · Arequipa · Perú</p>
          <h1 id="hero-title">Juega.<br /><em>Comparte.</em><br />Pertenece.</h1>
          <p class="hero-intro">{{ club.intro }}</p>
          <div class="hero-actions"><button class="button button-primary" type="button" @click="scrollToSection('#cancha')">Descubre la cancha <span aria-hidden="true">↓</span></button><a class="text-link" href="#comunidad">Conoce el club <span aria-hidden="true">↗</span></a></div>
        </div>
        <div class="hero-emblem" aria-hidden="true">
          <div class="orbit orbit-one"></div><div class="orbit orbit-two"></div><img ref="shield" src="/escudo-franciscos-club.png" alt="" /><span class="emblem-caption">F · C <span></span> 2019</span>
        </div>
        <div class="hero-bottom"><span>Scroll para explorar</span><span class="scroll-line"></span><span>01—06</span></div>
      </section>

      <section id="cancha" class="court-section section-padding" aria-labelledby="court-title">
        <div class="court-copy">
          <p class="eyebrow light">La cancha te espera</p>
          <h2 id="court-title">Haz que<br /><em>pase.</em></h2>
          <p class="court-intro">Entra, mueve la mirada y toca la cancha para soltar el remate. Una pequeña muestra del ritmo que te espera dentro.</p>
          <div class="price-lockup" aria-label="Precio de cancha: 50 soles">
            <span>Precio de cancha</span>
            <strong><small>S/</small> 50</strong>
            <em>Consulta condiciones y disponibilidad</em>
          </div>
          <a class="button button-gold" href="https://linktr.ee/franciscosclub" target="_blank" rel="noopener noreferrer">Consultar disponibilidad <span aria-hidden="true">↗</span></a>
        </div>
        <div class="court-visual">
          <Suspense><template #default><ThreeCourt /></template><template #fallback><div class="three-court-fallback">Preparando la cancha…</div></template></Suspense>
          <p class="court-caption"><span>Experiencia interactiva</span><span>Mueve el cursor · toca para patear</span></p>
        </div>
      </section>

      <section id="momentos" class="moments-section section-padding" aria-labelledby="moments-title">
        <div class="moments-heading">
          <div><p class="eyebrow">La experiencia completa</p><h2 id="moments-title">Más que<br /><em>un partido.</em></h2></div>
          <div class="moments-intro"><p>Hay algo que ocurre antes de que ruede la pelota y continúa mucho después del último toque.</p><span>Antes <i>→</i> Durante <i>→</i> Después</span></div>
        </div>
        <div class="moments-grid">
          <article v-for="(moment, index) in moments" :key="moment.moment" class="moment-card" :class="`moment-card--${index + 1}`">
            <div class="moment-image"><img :src="moment.image" :alt="moment.alt" loading="lazy" /><span>{{ moment.number }} · {{ moment.moment }}</span></div>
            <div class="moment-copy"><h3>{{ moment.title }}</h3><p>{{ moment.text }}</p></div>
          </article>
        </div>
      </section>

      <section id="quedarse" class="stay-section" aria-labelledby="stay-title">
        <div class="stay-visual">
          <img class="stay-main-image" src="/assets/quedarse-mesa-v4.jpg" alt="Cinco amigos compartiendo comida después de un partido junto a una cancha iluminada" loading="lazy" />
          <div class="stay-overlay" aria-hidden="true"></div>
          <figure class="stay-inset"><img src="/assets/quedarse-regreso-v4.jpg" alt="Cuatro amigos caminando de la cancha hacia la zona social del club" loading="lazy" /><figcaption>La noche apenas empieza</figcaption></figure>
        </div>
        <div class="stay-copy section-padding">
          <p class="eyebrow">El lado social del club</p>
          <h2 id="stay-title">Un lugar<br /><em>para quedarse.</em></h2>
          <p>El partido abre la conversación. Después llegan la mesa, la comida y ese tiempo sin apuro que transforma una visita en costumbre.</p>
          <ul class="stay-list">
            <li><span>01</span><strong>Una mesa después del partido</strong></li>
            <li><span>02</span><strong>Comida, barra y conversación</strong></li>
            <li><span>03</span><strong>Un ambiente cercano en Paucarpata</strong></li>
          </ul>
        </div>
      </section>

      <section id="oferta" class="offer-section section-padding" aria-labelledby="offer-title">
        <div class="offer-heading"><p class="eyebrow">Todo en un mismo lugar</p><h2 id="offer-title">Lo que encuentras<br /><em>aquí.</em></h2><p>De la cancha a la mesa, Francisco’s Club reúne deporte y encuentro en una experiencia sencilla y cercana.</p></div>
        <div class="offer-grid">
          <article v-for="item in offerings" :key="item.number" class="offer-card">
            <div class="offer-top"><span>{{ item.number }}</span><span>{{ item.tag }}</span></div>
            <div class="offer-image"><img :src="item.image" :alt="item.alt" loading="lazy" /></div>
            <h3>{{ item.title }}</h3><p>{{ item.text }}</p>
          </article>
        </div>
        <p class="offer-legal">Actividades declaradas: {{ club.activities[0] }} · {{ club.activities[1] }}</p>
      </section>

      <SocialCommunity />
    </main>

    <footer id="visitanos" class="site-footer">
      <div class="footer-main">
        <div class="footer-identity"><a class="brand footer-brand" href="#inicio"><span class="brand-mark"><img src="/escudo-franciscos-club.png" alt="" /></span><span class="brand-copy"><strong>FRANCISCO’S</strong><small>CLUB</small></span></a><p>{{ club.claim }}</p></div>
        <div class="footer-column"><span>Ubicación</span><p>{{ club.address }}<br />{{ club.addressDetail }}</p></div>
        <div class="footer-column"><span>Información legal</span><p>{{ club.legalName }}<br />RUC {{ club.ruc }} · {{ club.status }}<br />Inicio: {{ club.founded }}</p></div>
        <nav class="footer-column footer-nav" aria-label="Navegación del footer"><span>Explora</span><a href="#cancha">La cancha</a><a href="#momentos">La experiencia</a><a href="#oferta">Lo que encuentras</a><a href="#comunidad">Comunidad</a></nav>
      </div>
      <div class="footer-bottom"><span>Francisco’s Club · Paucarpata, Arequipa</span><span>Imágenes conceptuales generadas para comunicación visual</span><a href="#inicio">Volver arriba ↑</a></div>
    </footer>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Manrope:wght@400;500;600;700;800&family=Playfair+Display:ital,wght@0,600;1,600;1,700&display=swap');
:root { --ink:#101112; --cream:#f3efe5; --paper:#faf8f2; --gold:#c99a3b; --gold-light:#e7c06a; --muted:#827d70; --line:rgba(16,17,18,.14); --green:#1b3129; }
* { box-sizing:border-box; } html { scroll-behavior:smooth; } body { -webkit-font-smoothing:antialiased; margin:0; min-width:320px; background:var(--cream); color:var(--ink); font-family:'Manrope',sans-serif; line-height:1.5; text-rendering:optimizeLegibility; } button,a { font:inherit; } button { cursor:pointer; } a { color:inherit; text-decoration:none; } h1,h2,h3,p { margin-top:0; } main section,footer { scroll-margin-top:24px; } ::selection { background:var(--gold-light); color:var(--ink); }
.site-shell { overflow-x:clip; position:relative; } .grain { background-image:url('/assets/grain-static.png'); background-repeat:repeat; background-size:96px 96px; inset:0; opacity:.035; pointer-events:none; position:absolute; z-index:0; }
.site-header { align-items:center; display:flex; height:88px; justify-content:space-between; left:0; padding:0 5vw; position:absolute; right:0; top:0; z-index:4; } .brand { align-items:center; display:inline-flex; gap:11px; } .brand-mark { align-items:center; display:flex; height:44px; justify-content:center; width:38px; } .brand-mark img { filter:drop-shadow(0 4px 5px rgba(0,0,0,.15)); height:100%; object-fit:contain; width:100%; } .brand-copy { display:flex; flex-direction:column; line-height:.92; } .brand-copy strong { font-size:14px; font-weight:800; letter-spacing:-.04em; } .brand-copy small { font-family:'DM Mono',monospace; font-size:9px; letter-spacing:.38em; margin-left:2px; margin-top:4px; }
.main-nav { display:flex; gap:30px; margin-left:8vw; } .main-nav a { color:rgba(16,17,18,.64); font-size:11px; font-weight:700; padding-bottom:5px; position:relative; transition:color .2s; } .main-nav a::after { background:var(--gold); bottom:0; content:''; height:1px; left:0; position:absolute; transform:scaleX(0); transform-origin:left; transition:transform .25s ease; width:100%; } .main-nav a:hover,.text-link:hover { color:var(--gold); } .main-nav a:hover::after,.main-nav a:focus-visible::after { transform:scaleX(1); } .header-action { background:transparent; border:1px solid var(--ink); border-radius:100px; color:var(--ink); font-size:10px; font-weight:800; padding:12px 17px; transition:background .2s,color .2s,transform .2s; } .header-action span { font-size:14px; margin-left:7px; } .header-action:hover { background:var(--ink); color:var(--cream); transform:translateY(-2px); }
.hero { background:var(--cream); display:flex; min-height:850px; overflow:hidden; padding:170px 5vw 70px; position:relative; } .hero-content { max-width:610px; position:relative; z-index:2; } .eyebrow { align-items:center; color:var(--muted); display:flex; font-family:'DM Mono',monospace; font-size:10px; gap:9px; letter-spacing:.12em; margin:0 0 28px; text-transform:uppercase; } .eyebrow.light { color:rgba(243,239,229,.5); } .live-dot { background:var(--gold); border-radius:50%; box-shadow:0 0 0 5px rgba(201,154,59,.12); height:6px; width:6px; } h1,h2 { font-size:clamp(58px,7.6vw,112px); letter-spacing:-.075em; line-height:.89; margin-bottom:35px; } h1 em,h2 em { color:var(--gold); font-family:'Playfair Display',Georgia,serif; font-weight:600; letter-spacing:-.08em; } .hero-intro { color:rgba(16,17,18,.65); font-size:14px; line-height:1.75; max-width:335px; } .hero-actions { align-items:center; display:flex; gap:27px; margin-top:35px; }
.button { align-items:center; border:0; border-radius:100px; display:inline-flex; font-size:11px; font-weight:800; gap:24px; justify-content:center; padding:15px 19px; transition:transform .2s,background .2s; } .button:hover { transform:translateY(-2px); } .button span { font-size:18px; font-weight:400; line-height:.5; } .button-primary { background:var(--ink); color:var(--cream); } .button-primary:hover { background:var(--gold); } .button-gold { background:var(--gold-light); color:var(--ink); } .button-gold:hover { background:var(--cream); } .button-light { background:var(--cream); color:var(--ink); } .button-light:hover { background:var(--gold-light); } .text-link { border-bottom:1px solid rgba(16,17,18,.28); color:rgba(16,17,18,.7); font-size:11px; font-weight:700; padding-bottom:4px; transition:color .2s; }
.hero-emblem { align-items:center; display:flex; height:660px; justify-content:center; position:absolute; right:1.5vw; top:95px; width:min(52vw,720px); } .hero-emblem img { filter:none; max-height:520px; max-width:75%; object-fit:contain; position:relative; transition:none; will-change:transform; z-index:1; } .orbit { border:1px solid rgba(16,17,18,.13); border-radius:50%; height:600px; position:absolute; transform:rotate(-25deg) scaleX(.65); width:600px; } .orbit-one { border-left-color:transparent; border-top-color:var(--gold); } .orbit-two { height:510px; transform:rotate(48deg) scaleX(.8); width:510px; } .emblem-caption { bottom:42px; color:var(--muted); font-family:'DM Mono',monospace; font-size:9px; letter-spacing:.2em; position:absolute; right:14%; transform:rotate(-90deg); } .emblem-caption span { background:var(--gold); border-radius:50%; display:inline-block; height:4px; margin:0 8px; width:4px; } .hero-ambient { border-radius:50%; opacity:.7; position:absolute; } .ambient-left { background:rgba(211,169,79,.13); height:300px; left:-150px; top:430px; width:300px; } .ambient-right { background:rgba(226,190,107,.21); height:260px; right:21%; top:240px; width:260px; } .hero-bottom { align-items:center; bottom:40px; color:var(--muted); display:flex; font-family:'DM Mono',monospace; font-size:9px; gap:14px; left:5vw; letter-spacing:.12em; position:absolute; text-transform:uppercase; } .scroll-line { background:var(--gold); height:1px; width:55px; }
.section-padding { padding:145px 10vw; }
.court-section { background:var(--ink); color:var(--cream); display:grid; gap:7vw; grid-template-columns:.74fr 1.26fr; min-height:820px; overflow:hidden; position:relative; } .court-section::before { background:radial-gradient(circle,rgba(201,154,59,.22),transparent 65%); content:''; height:600px; position:absolute; right:4vw; top:60px; width:600px; } .court-copy { align-self:center; position:relative; z-index:2; } .court-copy h2 { font-size:clamp(58px,6.4vw,92px); } .court-copy h2 em { color:var(--gold-light); } .court-intro { color:rgba(243,239,229,.6); font-size:14px; line-height:1.8; max-width:340px; }
.price-lockup { border-bottom:1px solid rgba(243,239,229,.2); border-top:1px solid rgba(243,239,229,.2); display:grid; grid-template-columns:1fr auto; margin:39px 0 28px; max-width:380px; padding:18px 0; } .price-lockup > span,.price-lockup em { color:rgba(243,239,229,.48); font-family:'DM Mono',monospace; font-size:9px; font-style:normal; letter-spacing:.1em; text-transform:uppercase; } .price-lockup strong { color:var(--gold-light); font-size:58px; grid-row:span 2; letter-spacing:-.08em; line-height:.85; } .price-lockup strong small { font-size:18px; letter-spacing:0; } .price-lockup em { align-self:end; font-size:8px; }
.court-visual { align-self:center; height:620px; position:relative; z-index:1; } .court-visual .three-court { background:transparent; border:0; cursor:crosshair; height:100%; margin:0; min-height:0; overflow:hidden; position:relative; width:100%; } .court-visual .three-court::before { background:radial-gradient(circle at 58% 38%,rgba(231,192,106,.18),transparent 24%),linear-gradient(135deg,rgba(243,239,229,.04),transparent 58%); content:''; inset:0; pointer-events:none; position:absolute; z-index:1; } .court-caption { bottom:12px; color:rgba(243,239,229,.45); display:flex; font-family:'DM Mono',monospace; font-size:8px; justify-content:space-between; left:20px; letter-spacing:.1em; margin:0; position:absolute; right:20px; text-transform:uppercase; z-index:3; } .three-court canvas { display:block; height:100%; width:100%; } .three-court-label { bottom:38px; color:rgba(243,239,229,.72); display:flex; font-family:'DM Mono',monospace; font-size:9px; justify-content:space-between; left:24px; letter-spacing:.12em; position:absolute; right:24px; text-transform:uppercase; z-index:2; } .three-court-label span:last-child { color:var(--gold-light); } .three-court-coordinate { color:rgba(243,239,229,.3); font-family:'DM Mono',monospace; font-size:8px; letter-spacing:.1em; position:absolute; right:24px; text-transform:uppercase; top:24px; z-index:2; } .coordinate-two { bottom:56px; left:24px; right:auto; top:auto; }
.moments-section { background:var(--paper); } .moments-heading { align-items:flex-end; display:flex; justify-content:space-between; margin-bottom:85px; } .moments-heading h2 { font-size:clamp(58px,7vw,96px); margin:0; } .moments-intro { max-width:310px; } .moments-intro p { color:var(--muted); font-size:13px; line-height:1.8; } .moments-intro span { color:var(--gold); font-family:'DM Mono',monospace; font-size:9px; letter-spacing:.12em; text-transform:uppercase; } .moments-intro i { color:var(--ink); font-style:normal; margin:0 8px; }
.moments-grid { align-items:start; display:grid; gap:24px; grid-template-columns:.88fr 1.12fr .88fr; } .moment-card { transition:transform .35s ease; } .moment-card:hover { transform:translateY(-6px); } .moment-card--2 { margin-top:70px; } .moment-image { background:#ded8c9; height:390px; overflow:hidden; position:relative; } .moment-card--2 .moment-image { height:500px; } .moment-image::after { background:linear-gradient(180deg,transparent 52%,rgba(16,17,18,.74)); content:''; inset:0; position:absolute; } .moment-image img { display:block; height:100%; object-fit:cover; transition:transform .8s ease; width:100%; } .moment-card:hover img { transform:scale(1.045); } .moment-image span { bottom:18px; color:var(--cream); font-family:'DM Mono',monospace; font-size:9px; left:18px; letter-spacing:.12em; position:absolute; text-transform:uppercase; z-index:1; } .moment-copy { padding:24px 7px 0; } .moment-copy h3 { font-size:23px; letter-spacing:-.05em; line-height:1.15; margin-bottom:12px; } .moment-copy p { color:var(--muted); font-size:12px; line-height:1.75; max-width:270px; }
.stay-section { background:var(--cream); display:grid; grid-template-columns:1.08fr .92fr; min-height:820px; } .stay-visual { min-height:720px; overflow:hidden; position:relative; } .stay-main-image { height:100%; object-fit:cover; object-position:center 52%; width:100%; } .stay-overlay { background:linear-gradient(90deg,transparent 55%,rgba(16,17,18,.16)),linear-gradient(180deg,transparent 50%,rgba(16,17,18,.5)); inset:0; position:absolute; } .stay-inset { background:var(--paper); bottom:42px; margin:0; padding:8px 8px 14px; position:absolute; right:-45px; width:190px; z-index:2; } .stay-inset img { aspect-ratio:4/5; display:block; object-fit:cover; width:100%; } .stay-inset figcaption { color:var(--muted); font-family:'DM Mono',monospace; font-size:8px; letter-spacing:.1em; padding:11px 6px 0; text-transform:uppercase; }
.stay-copy { align-self:center; padding-left:9vw; padding-right:8vw; } .stay-copy h2 { font-size:clamp(54px,6vw,86px); } .stay-copy > p:not(.eyebrow) { color:var(--muted); font-size:14px; line-height:1.85; max-width:390px; } .stay-list { border-top:1px solid var(--line); list-style:none; margin:46px 0 0; max-width:410px; padding:0; } .stay-list li { align-items:center; border-bottom:1px solid var(--line); display:flex; gap:20px; padding:16px 0; } .stay-list span { color:var(--gold); font-family:'DM Mono',monospace; font-size:9px; } .stay-list strong { font-size:11px; font-weight:700; }
.offer-section { background:var(--gold); } .offer-heading { align-items:flex-end; display:grid; gap:5vw; grid-template-columns:auto 1fr 280px; margin-bottom:80px; } .offer-heading h2 { font-size:clamp(58px,7vw,96px); margin:0; } .offer-heading h2 em { color:var(--cream); } .offer-heading > p:last-child { color:rgba(16,17,18,.62); font-size:13px; line-height:1.8; }
.offer-grid { border-top:1px solid rgba(16,17,18,.22); display:grid; grid-template-columns:repeat(3,1fr); } .offer-card { border-right:1px solid rgba(16,17,18,.22); min-height:560px; padding:25px 32px 40px 0; } .offer-card + .offer-card { padding-left:32px; } .offer-card:last-child { border-right:0; } .offer-top { display:flex; font-family:'DM Mono',monospace; font-size:9px; justify-content:space-between; letter-spacing:.12em; text-transform:uppercase; } .offer-top span:last-child { color:rgba(16,17,18,.58); } .offer-image { aspect-ratio:4/3; background:rgba(16,17,18,.09); margin:30px 0 28px; overflow:hidden; } .offer-image img { display:block; height:100%; object-fit:cover; transition:filter .35s ease,transform .7s ease; width:100%; } .offer-card:hover .offer-image img { filter:saturate(1.08) contrast(1.03); transform:scale(1.035); } .offer-card h3 { font-size:23px; letter-spacing:-.05em; margin-bottom:13px; max-width:260px; } .offer-card p { color:rgba(16,17,18,.62); font-size:12px; line-height:1.75; max-width:245px; } .offer-legal { border-top:1px solid rgba(16,17,18,.22); color:rgba(16,17,18,.55); font-family:'DM Mono',monospace; font-size:8px; letter-spacing:.1em; margin:0; padding-top:18px; text-transform:uppercase; }
.site-footer { background:var(--ink); color:var(--cream); padding:78px 5vw 25px; } .footer-main { display:grid; gap:5vw; grid-template-columns:1.15fr 1fr 1fr .7fr; padding-bottom:70px; } .footer-brand .brand-mark { height:42px; width:36px; } .footer-identity p { color:rgba(243,239,229,.5); font-size:12px; line-height:1.7; margin:25px 0 0; max-width:240px; } .footer-column > span { color:var(--gold-light); display:block; font-family:'DM Mono',monospace; font-size:8px; letter-spacing:.12em; margin-bottom:18px; text-transform:uppercase; } .footer-column p,.footer-column a { color:rgba(243,239,229,.55); font-size:10px; line-height:1.9; } .footer-nav { display:flex; flex-direction:column; } .footer-nav a:hover { color:var(--gold-light); } .footer-bottom { align-items:center; border-top:1px solid rgba(243,239,229,.14); color:rgba(243,239,229,.35); display:flex; font-family:'DM Mono',monospace; font-size:7px; justify-content:space-between; letter-spacing:.08em; padding-top:24px; text-transform:uppercase; } .footer-bottom a { color:rgba(243,239,229,.65); }
.three-court-fallback { align-items:center; background:rgba(243,239,229,.04); color:rgba(243,239,229,.55); display:flex; font-family:'DM Mono',monospace; font-size:10px; height:100%; justify-content:center; letter-spacing:.12em; text-transform:uppercase; }
:focus-visible { outline:2px solid var(--gold); outline-offset:4px; }
@media (max-width:1100px) and (min-width:801px) {
  .site-header { padding:0 4vw; } .main-nav { gap:20px; margin-left:2vw; }
  .section-padding { padding:120px 6vw; }
  .court-section { gap:3vw; grid-template-columns:.8fr 1.2fr; } .court-visual { height:540px; }
  .stay-copy { padding-left:7vw; padding-right:5vw; }
  .offer-heading { gap:3vw; grid-template-columns:.75fr 1.5fr .85fr; }
}
@media (max-width:800px) {
  .grain { display:none; }
  .site-header { height:75px; padding:0 6vw; } .main-nav { display:none; } .header-action { font-size:9px; padding:10px 12px; }
  .hero { min-height:820px; padding:135px 8vw 80px; } .hero-content { max-width:100%; } .hero h1 { font-size:clamp(51px,15vw,68px); } .hero-intro { max-width:300px; } .hero-emblem { bottom:-48px; height:390px; opacity:.82; right:-115px; top:auto; width:470px; } .hero-emblem img { max-height:300px; } .orbit { height:400px; width:400px; } .orbit-two { height:340px; width:340px; } .hero-bottom { bottom:22px; left:8vw; }
  .section-padding { padding:95px 8vw; }
  .court-section { display:block; min-height:0; } .court-copy { margin-bottom:65px; } .court-visual { height:470px; margin:0 -18vw; } .court-caption { display:none; } .price-lockup { max-width:100%; }
  .moments-heading { align-items:flex-start; display:block; margin-bottom:55px; } .moments-intro { margin-top:30px; max-width:100%; } .moments-grid { display:block; } .moment-card,.moment-card--2 { margin:0 0 46px; } .moment-image,.moment-card--2 .moment-image { height:390px; }
  .stay-section { display:block; } .stay-visual { min-height:560px; } .stay-inset { bottom:24px; right:18px; width:150px; } .stay-copy { padding:95px 8vw; }
  .offer-heading { display:block; margin-bottom:55px; } .offer-heading h2 { margin:0 0 27px; } .offer-grid { display:block; } .offer-card,.offer-card + .offer-card { border-bottom:1px solid rgba(16,17,18,.22); border-right:0; min-height:0; padding:24px 0 35px; } .offer-image { margin:26px 0 25px; }
  .footer-main { grid-template-columns:1fr; } .footer-bottom { align-items:flex-start; flex-direction:column; gap:12px; }
  .three-court-label { bottom:28px; flex-direction:column; gap:6px; left:15px; right:auto; } .three-court-coordinate { right:15px; top:15px; } .coordinate-two { bottom:53px; left:auto; right:15px; top:auto; }
}
@media (max-width:520px) {
  .hero-actions { align-items:flex-start; flex-direction:column; gap:18px; }
  .court-copy h2,.moments-heading h2,.stay-copy h2,.offer-heading h2 { font-size:clamp(47px,14vw,63px); }
  .court-visual { height:400px; margin-left:-32vw; margin-right:-32vw; }
  .moment-image,.moment-card--2 .moment-image { height:340px; }
  .stay-visual { min-height:500px; } .stay-inset { width:128px; }
  .price-lockup strong { font-size:52px; }
}
@media (prefers-reduced-motion:reduce) { html { scroll-behavior:auto; } *,*::before,*::after { scroll-behavior:auto !important; transition-duration:.01ms !important; } }
</style>
