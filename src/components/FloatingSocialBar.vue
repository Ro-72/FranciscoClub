<script setup>
import { faFacebookF, faTiktok, faWhatsapp } from '@fortawesome/free-brands-svg-icons'
import { faStar } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

const socialLinks = [
  {
    label: 'Grupo de Pichangueros',
    ariaLabel: 'Unirse al grupo de WhatsApp de Pichangueros',
    href: 'https://chat.whatsapp.com/FQ86A5S796vKi5fQnJa1kY',
    icon: faWhatsapp,
  },
  {
    label: 'Déjanos una reseña',
    ariaLabel: "Dejar una reseña de Francisco's Club en Google",
    href: 'https://g.page/r/CYlJ6j8EZGW2EAI/review',
    icon: faStar,
  },
  {
    label: 'Síguenos en TikTok',
    ariaLabel: "Visitar TikTok de Francisco's Club",
    href: 'https://www.tiktok.com/@franciscosclub',
    icon: faTiktok,
  },
  {
    label: 'Síguenos en Facebook',
    ariaLabel: "Visitar Facebook de Francisco's Club",
    href: 'https://www.facebook.com/canchadegrasssinteticoarequipa/',
    icon: faFacebookF,
  },
]
</script>

<template>
  <aside class="floating-social" aria-label="Accesos rápidos a redes sociales">
    <span class="floating-social-line" aria-hidden="true"></span>
    <nav class="floating-social-links" aria-label="Redes sociales de Francisco's Club">
      <a
        v-for="item in socialLinks"
        :key="item.ariaLabel"
        class="floating-social-link"
        :href="item.href"
        target="_blank"
        rel="noopener noreferrer"
        :aria-label="item.ariaLabel"
      >
        <FontAwesomeIcon class="floating-social-icon" :icon="item.icon" aria-hidden="true" />
        <span class="floating-social-tooltip" aria-hidden="true">{{ item.label }}</span>
      </a>
    </nav>
    <span class="floating-social-line" aria-hidden="true"></span>
  </aside>
</template>

<style scoped>
.floating-social {
  --floating-cream: #f4f0e6;
  --floating-black: #101010;
  --floating-gold: #d5a32a;
  --floating-green: #173f35;
  align-items: center;
  display: flex;
  flex-direction: column;
  gap: 12px;
  left: 12px;
  position: fixed;
  top: 50%;
  transform: translateY(-50%);
  z-index: 1000;
}

.floating-social-line { background: var(--floating-gold); display: block; height: 38px; opacity: .82; width: 1px; }
.floating-social-links { display: flex; flex-direction: column; gap: 8px; }
.floating-social-link {
  align-items: center;
  background: var(--floating-black);
  border: 1px solid rgba(244,240,230,.16);
  border-radius: 50%;
  color: var(--floating-cream);
  display: flex;
  height: 46px;
  justify-content: center;
  position: relative;
  transition: background-color .25s ease, color .25s ease, transform .25s ease;
  width: 46px;
}

.floating-social-icon { font-size: 21px; }
.floating-social-tooltip {
  background: var(--floating-black);
  border-left: 2px solid var(--floating-gold);
  color: var(--floating-cream);
  font-family: 'DM Mono', monospace;
  font-size: 8px;
  left: calc(100% + 13px);
  letter-spacing: .08em;
  opacity: 0;
  padding: 9px 12px;
  pointer-events: none;
  position: absolute;
  text-transform: uppercase;
  top: 50%;
  transform: translate(-5px,-50%);
  transition: opacity .25s ease, transform .25s ease, visibility .25s ease;
  visibility: hidden;
  white-space: nowrap;
}

.floating-social-link:hover,
.floating-social-link:focus { background: var(--floating-gold); color: var(--floating-black); transform: scale(1.08); }
.floating-social-link:hover .floating-social-tooltip,
.floating-social-link:focus .floating-social-tooltip,
.floating-social-link:focus-visible .floating-social-tooltip { opacity: 1; transform: translate(0,-50%); visibility: visible; }
.floating-social-link:focus { outline: none; }
.floating-social-link:focus-visible { outline: 3px solid var(--floating-cream); outline-offset: 3px; }

@media (max-width: 1100px) and (min-width: 681px) {
  .floating-social { left: 6px; }
  .floating-social-link { height: 40px; width: 40px; }
  .floating-social-icon { font-size: 19px; }
  .floating-social-line { height: 30px; }
}

@media (max-width: 680px) {
  .floating-social {
    background: var(--floating-black);
    border: 1px solid rgba(244,240,230,.2);
    border-radius: 100px;
    bottom: 16px;
    flex-direction: row;
    gap: 0;
    left: 50%;
    padding: 5px;
    top: auto;
    transform: translateX(-50%);
  }
  .floating-social-line { display: none; }
  .floating-social-links { flex-direction: row; gap: 4px; }
  .floating-social-link { border-color: transparent; height: 43px; width: 43px; }
  .floating-social-icon { font-size: 19px; }
  .floating-social-tooltip { display: none; }
}

@media (prefers-reduced-motion: reduce) {
  .floating-social-link,
  .floating-social-tooltip { transition-duration: .01ms !important; }
}
</style>
