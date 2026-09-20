<script setup>
import { faFacebookF, faTiktok, faWhatsapp } from '@fortawesome/free-brands-svg-icons'
import { faStar } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

const socialLinks = [
  {
    number: '01',
    platform: 'WhatsApp',
    eyebrow: 'Pichangueros',
    title: 'Únete al grupo',
    description: 'Encuentra gente para jugar, coordina partidos y no te quedes sin pichanga.',
    cta: 'Entrar al grupo',
    href: 'https://chat.whatsapp.com/FQ86A5S796vKi5fQnJa1kY',
    ariaLabel: "Unirse al grupo de WhatsApp de Francisco's Club",
    icon: faWhatsapp,
    variant: 'whatsapp',
  },
  {
    number: '02',
    platform: 'Google',
    eyebrow: 'Tu experiencia',
    title: 'Déjanos una recomendación',
    description: '¿La pasaste bien? Tu opinión nos ayuda a seguir mejorando el club.',
    cta: 'Escribir reseña',
    href: 'https://g.page/r/CYlJ6j8EZGW2EAI/review',
    ariaLabel: "Dejar una reseña de Francisco's Club en Google",
    icon: faStar,
    variant: 'google',
  },
  {
    number: '03',
    platform: 'TikTok',
    eyebrow: 'Mira las jugadas',
    title: 'Síguenos en TikTok',
    description: 'Partidos, jugadas, momentos del club y contenido de nuestra comunidad.',
    cta: 'Ver TikTok',
    href: 'https://www.tiktok.com/@franciscosclub',
    ariaLabel: "Visitar TikTok de Francisco's Club",
    icon: faTiktok,
    variant: 'tiktok',
  },
  {
    number: '04',
    platform: 'Facebook',
    eyebrow: 'Comunidad',
    title: 'Encuéntranos en Facebook',
    description: "Noticias, fotos, novedades y momentos de Francisco's Club.",
    cta: 'Ver Facebook',
    href: 'https://www.facebook.com/canchadegrasssinteticoarequipa/',
    ariaLabel: "Visitar Facebook de Francisco's Club",
    icon: faFacebookF,
    variant: 'facebook',
  },
]
</script>

<template>
  <section id="comunidad" class="social-community" aria-labelledby="social-community-title">
    <header class="social-heading">
      <p class="social-kicker">Comunidad</p>
      <div class="social-heading-grid">
        <h2 id="social-community-title">Fuera de la cancha,<br /><em>seguimos conectados.</em></h2>
        <p>Únete a la comunidad, comparte tu experiencia y acompáñanos también fuera de la cancha.</p>
      </div>
    </header>

    <div class="social-grid" aria-label="Redes sociales y comunidad de Francisco's Club">
      <a
        v-for="item in socialLinks"
        :key="item.number"
        class="social-card"
        :class="`social-card--${item.variant}`"
        :href="item.href"
        target="_blank"
        rel="noopener noreferrer"
        :aria-label="item.ariaLabel"
      >
        <div class="social-card-top">
          <span>{{ item.number }}</span>
          <span>{{ item.platform }}</span>
        </div>
        <FontAwesomeIcon class="social-icon" :icon="item.icon" aria-hidden="true" />
        <div class="social-card-copy">
          <p class="social-eyebrow">{{ item.eyebrow }}</p>
          <h3>{{ item.title }}</h3>
          <p class="social-description">{{ item.description }}</p>
        </div>
        <span class="social-cta">{{ item.cta }} <b aria-hidden="true">↗</b></span>
      </a>
    </div>

    <div class="social-closing">
      <div>
        <span>La próxima historia comienza aquí</span>
        <h3>Nos vemos en la cancha.</h3>
      </div>
      <a
        class="location-link"
        href="https://www.google.com/maps/search/?api=1&query=Paucarpata%20Arequipa"
        target="_blank"
        rel="noopener noreferrer"
      >Cómo llegar <span aria-hidden="true">↗</span></a>
    </div>
  </section>
</template>

<style scoped>
.social-community {
  --social-background: #f4f0e6;
  --social-black: #101010;
  --social-gold: #d5a32a;
  --social-green: #173f35;
  --social-gray: #77736b;
  background: var(--social-background);
  color: var(--social-black);
  padding: 140px 5vw 72px;
}

.social-heading { margin: 0 auto 76px; max-width: 1440px; }
.social-kicker { color: var(--social-gray); font-family: 'DM Mono', monospace; font-size: 10px; letter-spacing: .18em; margin: 0 0 30px; text-transform: uppercase; }
.social-heading-grid { align-items: end; display: grid; gap: 8vw; grid-template-columns: minmax(0, 1.45fr) minmax(260px, .55fr); }
.social-heading h2 { font-size: clamp(54px, 6.8vw, 98px); letter-spacing: -.075em; line-height: .9; margin: 0; }
.social-heading h2 em { color: var(--social-gold); font-family: 'Playfair Display', Georgia, serif; font-weight: 600; }
.social-heading-grid > p { color: var(--social-gray); font-size: 14px; line-height: 1.85; margin: 0 0 8px; max-width: 360px; }

.social-grid { border-bottom: 1px solid rgba(16,16,16,.18); border-top: 1px solid rgba(16,16,16,.18); display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); margin: 0 auto; max-width: 1440px; }
.social-card { background: transparent; border-right: 1px solid rgba(16,16,16,.18); color: var(--social-black); display: flex; flex-direction: column; min-height: 360px; padding: 24px 26px 27px; transition: background-color .32s ease, color .32s ease; }
.social-card:last-child { border-right: 0; }
.social-card-top { color: var(--social-gray); display: flex; font-family: 'DM Mono', monospace; font-size: 8px; justify-content: space-between; letter-spacing: .14em; text-transform: uppercase; transition: color .32s ease; }
.social-icon { color: var(--social-black); font-size: 34px; margin: 43px 0 35px; transform-origin: left center; transition: color .32s ease, transform .32s ease; }
.social-card-copy { flex: 1; }
.social-eyebrow { color: var(--social-gold); font-family: 'DM Mono', monospace; font-size: 8px; letter-spacing: .16em; margin: 0 0 12px; text-transform: uppercase; }
.social-card h3 { font-size: clamp(20px, 1.7vw, 27px); letter-spacing: -.055em; line-height: 1.08; margin: 0 0 15px; max-width: 245px; }
.social-description { color: var(--social-gray); font-size: 11px; line-height: 1.72; margin: 0; max-width: 240px; transition: color .32s ease; }
.social-cta { align-items: center; display: flex; font-family: 'DM Mono', monospace; font-size: 8px; font-weight: 500; justify-content: space-between; letter-spacing: .12em; margin-top: 28px; text-transform: uppercase; transition: color .32s ease; }
.social-cta b { font-size: 17px; font-weight: 400; transition: transform .32s ease; }

.social-card:hover,
.social-card:focus-visible { background: var(--social-black); color: var(--social-background); }
.social-card--whatsapp:hover,
.social-card--whatsapp:focus-visible { background: var(--social-green); }
.social-card:hover .social-card-top,
.social-card:focus-visible .social-card-top,
.social-card:hover .social-description,
.social-card:focus-visible .social-description { color: rgba(244,240,230,.67); }
.social-card:hover .social-icon,
.social-card:focus-visible .social-icon,
.social-card:hover .social-cta,
.social-card:focus-visible .social-cta { color: var(--social-gold); }
.social-card:hover .social-icon,
.social-card:focus-visible .social-icon { transform: scale(1.08); }
.social-card:hover .social-cta b,
.social-card:focus-visible .social-cta b { transform: translate(3px,-3px); }
.social-card:focus-visible { outline: 3px solid var(--social-gold); outline-offset: -3px; position: relative; z-index: 1; }

.social-closing { align-items: center; border-top: 1px solid rgba(16,16,16,.18); display: flex; justify-content: space-between; margin: 72px auto 0; max-width: 1440px; padding-top: 44px; }
.social-closing > div > span { color: var(--social-gray); font-family: 'DM Mono', monospace; font-size: 8px; letter-spacing: .14em; text-transform: uppercase; }
.social-closing h3 { font-size: clamp(34px, 4vw, 58px); letter-spacing: -.065em; margin: 9px 0 0; }
.location-link { align-items: center; background: var(--social-black); border-radius: 100px; color: var(--social-background); display: inline-flex; font-size: 11px; font-weight: 800; gap: 25px; padding: 15px 20px; transition: background-color .28s ease, transform .28s ease; }
.location-link:hover { background: var(--social-green); transform: translateY(-2px); }
.location-link:focus-visible { outline: 3px solid var(--social-gold); outline-offset: 4px; }
.location-link span { font-size: 17px; }

@media (max-width: 1100px) {
  .social-community { padding: 120px 6vw 68px; }
  .social-heading-grid { gap: 5vw; grid-template-columns: 1.2fr .8fr; }
  .social-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  .social-card { border-bottom: 1px solid rgba(16,16,16,.18); min-height: 330px; }
  .social-card:nth-child(2n) { border-right: 0; }
  .social-card:nth-child(n+3) { border-bottom: 0; }
}

@media (max-width: 680px) {
  .social-community { padding: 92px 8vw 58px; }
  .social-heading { margin-bottom: 52px; }
  .social-heading-grid { display: block; }
  .social-heading h2 { font-size: clamp(46px, 13.5vw, 64px); }
  .social-heading-grid > p { margin-top: 30px; }
  .social-grid { display: block; }
  .social-card,
  .social-card:nth-child(n) { border-bottom: 1px solid rgba(16,16,16,.18); border-right: 0; min-height: 285px; padding: 22px 20px 24px; }
  .social-card:last-child { border-bottom: 0; }
  .social-icon { margin: 30px 0 26px; }
  .social-card h3 { font-size: 25px; max-width: none; }
  .social-description { max-width: 330px; }
  .social-closing { align-items: flex-start; flex-direction: column; gap: 30px; margin-top: 58px; }
}

@media (prefers-reduced-motion: reduce) {
  .social-card,
  .social-card *,
  .location-link { transition-duration: .01ms !important; }
}
</style>
