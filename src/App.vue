<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'

const apiUrl = import.meta.env.VITE_CHALET_API_URL || '/api/chalet'
const language = ref('fr')
const chaletData = ref(null)
const loading = ref(true)
const error = ref('')
const gallery = ref(null)
const villageSeason = ref('winter')

const copy = {
  fr: {
    bedrooms: 'Chambres',
    navRooms: 'Les chambres',
    navExperience: 'Le châlet',
    navContact: 'Nous trouver',
    badge: 'Le Grand-Bornand · Haute-Savoie',
    intro: 'Un refuge alpin, tout simplement.',
    alpineTitle: 'Au cœur des Alpes',
    alpineText: 'Situé sur la commune du Grand-Bornand, à 1 heure de Genève et 2h30 de Lyon.',
    directions: "S'y rendre :",
    aboutTitle: 'Le chalet',
    aboutTextOne: 'Le chalet vous offre un endroit de détente sur son alpage avec vue sur la vallée du Grand-Bornand et la chaîne des Aravis.',
    aboutTextTwo: 'Confortable et avec de nombreuses commodités, vous avez la possibilité de réserver les chambres à la nuitée ou le chalet complet.',
    villageTitle: 'Le Grand-Bornand',
    villageTextOne: 'Station touristique et village de charme, le Grand-Bornand vit toute l’année dans son écrin des Alpes, dominé par la chaîne des Aravis.',
    villageTextTwo: 'Profitez de sa station de ski alpin en hiver, de son centre aquatique ou de ses itinéraires de randonnée et de VTT en été.',
    discoverVillage: 'Découvrir le Grand-Bornand',
    villageWinterAlt: 'Le village du Grand-Bornand sous la neige, de nuit',
    villageSummerAlt: 'Le village du Grand-Bornand en été',
    switchToWinter: 'Voir la vue d’hiver',
    switchToSummer: 'Voir la vue d’été',
    birdTitle: 'Le Gypaète Barbu',
    birdText: 'Avec de la chance ou un bon œil, vous pourrez apercevoir dans le ciel le Gypaète Barbu, espèce protégée dont la réintroduction sur le territoire Haut-Savoyard débute en 1987, avec une première nidification réussie en 1997 sur la chaîne du Bargy, à 20 minutes du Grand-Bornand.',
    birdAlt: 'Un gypaète barbu en vol',
    roomsEyebrow: 'Nos chambres',
    roomsTitle: 'Votre coin de montagne.',
    roomsIntro: 'Des chambres pensées pour ralentir, respirer et se sentir chez soi.',
    people: 'personnes',
    forPeople: 'Pour',
    peopleUpTo: 'Jusqu’à',
    accessible: 'Accessible PMR',
    terrace: 'Terrasse',
    balcony: 'Balcon',
    bath: 'Baignoire',
    shower: 'Douche',
    bathrooms: 'salle(s) de bain',
    toilets: 'WC',
    tv: 'Télévision',
    wifi: 'Wi-Fi',
    beds: 'Couchages',
    bedWidth: 'Largeur',
    views: 'La vue',
    included: 'Les petits plus',
    optional: 'En supplément',
    pets: 'Nos amis les animaux',
    permitted: 'Animaux bienvenus',
    notPermitted: 'Animaux non autorisés',
    from: 'À partir de',
    perNight: '/ nuit',
    minimum: 'séjour minimum',
    baseRateForFirst: 'Tarif de base pour les',
    firstNights: 'premières nuitées',
    afterFirstNights: 'Au-delà de ces nuitées',
    nightRate: 'par nuit',
    availability: 'Quelques disponibilités',
    availabilityIntro: 'Les tarifs varient selon la période de votre séjour.',
    contactEyebrow: 'On vous attend',
    contactTitle: 'La montagne n’attend que vous.',
    contactText: 'Une question ou envie de réserver ? Écrivez-nous, nous serons ravis de vous répondre.',
    address: 'Nous trouver',
    email: 'Nous écrire',
    call: 'Nous appeler',
    book: 'Réserver votre séjour',
    more: 'Découvrir',
    loading: 'Le chalet ouvre ses portes…',
    loadingHint: 'Nous préparons votre arrivée.',
    errorTitle: 'Le chalet est momentanément inaccessible.',
    errorHint: 'Impossible de charger les informations pour le moment. Réessayez dans quelques instants.',
    retry: 'Réessayer',
    openGallery: 'Ouvrir la galerie',
    closeGallery: 'Fermer la galerie',
    previousPhoto: 'Photo précédente',
    nextPhoto: 'Photo suivante',
    photoOf: 'sur',
    periodWeek: 'semaine',
    periodNight: 'nuit',
    periodDay: 'jour',
    periodPerson: 'personne',
    periodStay: 'séjour',
    perPerson: 'par personne',
    footer: 'Un châlet de montagne, au cœur des Aravis.',
  },
  en: {
    bedrooms: 'Bedrooms',
    navRooms: 'The rooms',
    navExperience: 'The chalet',
    navContact: 'Find us',
    badge: 'Le Grand-Bornand · Haute-Savoie',
    intro: 'A little alpine hideaway.',
    alpineTitle: 'In the heart of the Alps',
    alpineText: 'In the commune of Le Grand-Bornand, one hour from Geneva and two and a half hours from Lyon.',
    directions: 'Get directions',
    aboutTitle: 'About the chalet',
    aboutTextOne: 'The chalet is a peaceful retreat on its mountain pasture, overlooking the Grand-Bornand valley and the Aravis mountain range.',
    aboutTextTwo: 'Comfortable and full of thoughtful amenities, you can book a room by the night or reserve the entire chalet.',
    villageTitle: 'Le Grand-Bornand',
    villageTextOne: 'A charming village and year-round destination, Le Grand-Bornand nestles in the Alps beneath the Aravis mountain range.',
    villageTextTwo: 'Enjoy the alpine ski area in winter, and the aquatic centre, hiking trails and mountain-bike routes in summer.',
    discoverVillage: 'Discover Le Grand-Bornand',
    villageWinterAlt: 'Le Grand-Bornand village beneath the snow at night',
    villageSummerAlt: 'Le Grand-Bornand village in summer',
    switchToWinter: 'See the winter view',
    switchToSummer: 'See the summer view',
    birdTitle: 'The Bearded Vulture',
    birdText: 'With a little luck and a keen eye, you may spot a bearded vulture in the sky. This protected species was reintroduced to Haute-Savoie starting in 1987, and successfully nested for the first time in 1997 in the Bargy mountain range, just 20 minutes from Le Grand-Bornand.',
    birdAlt: 'A bearded vulture in flight',
    roomsEyebrow: 'Our rooms',
    roomsTitle: 'Your own mountain escape.',
    roomsIntro: 'Thoughtful rooms made for slowing down, breathing deep and feeling at home.',
    people: 'guests',
    forPeople: 'For',
    peopleUpTo: 'Up to',
    accessible: 'Wheelchair accessible',
    terrace: 'Terrace',
    balcony: 'Balcony',
    bath: 'Bathtub',
    shower: 'Shower',
    bathrooms: 'bathroom(s)',
    toilets: 'WC',
    tv: 'Television',
    wifi: 'Wi-Fi',
    beds: 'Sleeping arrangements',
    bedWidth: 'Width',
    views: 'The view',
    included: 'Included comforts',
    optional: 'Optional extras',
    pets: 'Four-legged guests',
    permitted: 'Pets welcome',
    notPermitted: 'Pets not permitted',
    from: 'From',
    perNight: '/ night',
    minimum: 'minimum stay',
    baseRateForFirst: 'Base rate applies for the first',
    firstNights: 'nights',
    afterFirstNights: 'For each following night',
    nightRate: 'per night',
    availability: 'A few available dates',
    availabilityIntro: 'Rates vary depending on when you stay.',
    contactEyebrow: 'Come on in',
    contactTitle: 'The mountains are calling.',
    contactText: 'Have a question or ready to book? Send us a note — we would love to hear from you.',
    address: 'Find us',
    email: 'Send us a note',
    call: 'Give us a call',
    book: 'Book your stay',
    more: 'Discover',
    loading: 'The chalet is opening its doors…',
    loadingHint: 'We are getting everything ready.',
    errorTitle: 'The chalet is temporarily unavailable.',
    errorHint: 'We could not load the details just now. Please try again in a moment.',
    retry: 'Try again',
    openGallery: 'Open photo gallery',
    closeGallery: 'Close gallery',
    previousPhoto: 'Previous photo',
    nextPhoto: 'Next photo',
    photoOf: 'of',
    periodWeek: 'week',
    periodNight: 'night',
    periodDay: 'day',
    periodPerson: 'person',
    periodStay: 'stay',
    perPerson: 'per person',
    footer: 'A mountain chalet in the heart of the Aravis.',
  },
}

const text = computed(() => copy[language.value])
const chalet = computed(() => chaletData.value?.chalet ?? {})
const rooms = computed(() => chaletData.value?.chambres ?? [])
const info = computed(() => chaletData.value?.infos ?? {})
const chaletPhotos = computed(() => chalet.value.photos ?? [])

function localized(value) {
  if (typeof value === 'string') return value
  if (!value || typeof value !== 'object') return ''
  return value[language.value] || value[language.value === 'fr' ? 'en' : 'fr'] || ''
}

function safeUrl(value) {
  if (typeof value !== 'string') return ''
  try {
    const url = new URL(value, window.location.origin)
    return ['http:', 'https:', 'mailto:', 'tel:'].includes(url.protocol) ? url.href : ''
  } catch {
    return ''
  }
}

function formatPrice(value) {
  const amount = Number(value)
  if (!Number.isFinite(amount)) return ''
  return new Intl.NumberFormat(language.value === 'fr' ? 'fr-FR' : 'en-GB', {
    style: 'currency',
    currency: 'EUR',
    maximumFractionDigits: 0,
  }).format(amount)
}

function periodLabel(value) {
  const labels = {
    semaine: text.value.periodWeek,
    week: text.value.periodWeek,
    nuit: text.value.periodNight,
    night: text.value.periodNight,
    jour: text.value.periodDay,
    day: text.value.periodDay,
    personne: text.value.periodPerson,
    person: text.value.periodPerson,
  }
  return labels[value] || value || text.value.periodStay
}

function formattedDate(value) {
  const date = new Date(`${value}T12:00:00`)
  if (Number.isNaN(date.getTime())) return value
  return new Intl.DateTimeFormat(language.value === 'fr' ? 'fr-FR' : 'en-GB', {
    day: 'numeric',
    month: 'short',
    year: 'numeric',
  }).format(date)
}

function photoUrl(photo, thumbnail = false) {
  return safeUrl(thumbnail ? photo?.thumbnailUrl || photo?.url : photo?.url)
}

function validPhotos(photos) {
  return (photos ?? []).filter((photo) => photoUrl(photo))
}

function openGallery(photos, index, title) {
  const items = validPhotos(photos)
  if (!items.length) return
  gallery.value = { photos: items, index, title }
}

function closeGallery() {
  gallery.value = null
}

function moveGallery(direction) {
  if (!gallery.value || gallery.value.photos.length < 2) return
  const count = gallery.value.photos.length
  gallery.value.index = (gallery.value.index + direction + count) % count
}

function handleGalleryKeys(event) {
  if (!gallery.value) return
  if (event.key === 'Escape') closeGallery()
  if (event.key === 'ArrowLeft') moveGallery(-1)
  if (event.key === 'ArrowRight') moveGallery(1)
}

function nonEmptyConditions(conditions) {
  return (conditions ?? []).filter((item) => localized(item.condition).trim())
}

async function loadChalet() {
  loading.value = true
  error.value = ''
  try {
    const response = await fetch(apiUrl, { headers: { Accept: 'application/json' } })
    if (!response.ok) throw new Error(`Chalet request failed: ${response.status}`)
    const data = await response.json()
    if (!data || typeof data !== 'object' || !data.chalet) {
      throw new Error('The chalet response has an invalid format')
    }
    chaletData.value = data
  } catch (cause) {
    error.value = cause instanceof Error ? cause.message : 'Unknown request error'
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  loadChalet()
  window.addEventListener('keydown', handleGalleryKeys)
})

onUnmounted(() => window.removeEventListener('keydown', handleGalleryKeys))
</script>

<template>
  <div class="site-shell">
    <header class="site-header">
      <a
        class="brand"
        href="#"
        :aria-label="`${chalet.nom || 'Chalet le Gypaète'} — ${language === 'fr' ? 'accueil' : 'home'}`"
      >
        <img class="brand-mark" src="/images/gypaete-draw-sd.png" alt="" />
        <span class="brand-name">{{ chalet.nom || 'Le Gypaète' }}</span>
      </a>

      <nav class="main-nav" :aria-label="language === 'fr' ? 'Navigation principale' : 'Main navigation'">
        <a href="#chalet">{{ text.navExperience }}</a>
        <a href="#chambres">{{ text.navRooms }}</a>
        <a href="#contact">{{ text.navContact }}</a>
      </nav>

      <button
        class="language-switch"
        type="button"
        :aria-label="language === 'fr' ? 'Switch to English' : 'Passer en français'"
        @click="language = language === 'fr' ? 'en' : 'fr'"
      >
        <span class="language-globe" aria-hidden="true">◎</span>
        {{ language === 'fr' ? 'EN' : 'FR' }}
      </button>
    </header>

    <main>
      <section v-if="loading" class="state-panel" aria-live="polite">
        <span class="loader" aria-hidden="true"></span>
        <h1>{{ text.loading }}</h1>
        <p>{{ text.loadingHint }}</p>
      </section>

      <section v-else-if="error" class="state-panel error-panel" aria-live="polite">
        <span class="state-icon" aria-hidden="true">↗</span>
        <h1>{{ text.errorTitle }}</h1>
        <p>{{ text.errorHint }}</p>
        <button class="button button-dark" type="button" @click="loadChalet">{{ text.retry }}</button>
      </section>

      <template v-else>
        <section id="chalet" class="hero">
          <div class="hero-copy">
            <p class="eyebrow"><span class="eyebrow-line"></span>{{ text.badge }}</p>
            <h1>{{ chalet.nom || 'Chalet le Gypaète' }}</h1>
            <p class="hero-tagline">{{ text.intro }}</p>
            <div class="hero-description">{{ localized(chalet.description) }}</div>
            <a class="button button-dark" href="#chambres">
              {{ text.more }}
              <span aria-hidden="true">↘</span>
            </a>
          </div>

          <div class="hero-visual">
            <button
              v-if="photoUrl(chaletPhotos[0])"
              class="gallery-image-button"
              type="button"
              :aria-label="`${text.openGallery}: ${chalet.nom || 'Chalet le Gypaète'}`"
              @click="openGallery(chaletPhotos, 0, chalet.nom || 'Chalet le Gypaète')"
            >
              <img class="hero-image" :src="photoUrl(chaletPhotos[0])" :alt="chalet.nom || 'Chalet le Gypaète'" fetchpriority="high" />
            </button>
            <div v-else class="hero-image hero-image-placeholder" aria-hidden="true">
              <span>LES ARAVIS</span>
              <svg viewBox="0 0 700 650" preserveAspectRatio="xMidYMid slice">
                <path d="m-40 470 160-235 108 146 128-232 103 150 84-94 202 286v180H-40Z" />
                <path d="m141 266-21-31 31 16 31-23 23 43-34-12-20 23ZM335 203l21-54 21 41 28-14 15 34-44-13-24 23Z" />
              </svg>
            </div>
            <button
              v-if="validPhotos(chaletPhotos).length > 1"
              class="gallery-count hero-gallery-count"
              type="button"
              :aria-label="`${text.openGallery}: ${validPhotos(chaletPhotos).length} ${text.photoOf} ${validPhotos(chaletPhotos).length}`"
              @click="openGallery(chaletPhotos, 0, chalet.nom || 'Chalet le Gypaète')"
            >▧ {{ validPhotos(chaletPhotos).length }} {{ language === 'fr' ? 'photos' : 'photos' }}</button>
            <div class="photo-caption">
              <span>01 — {{ chalet.nom || 'Chalet le Gypaète' }}</span>
              <span>LES ARAVIS, FRANCE</span>
            </div>
            <div class="image-stamp" aria-hidden="true">
              <span>ALTITUDE</span>
              <strong>1 300</strong>
              <span>MÈTRES</span>
            </div>
          </div>
        </section>

        <section class="intro-strip">
          <div class="intro-mark" aria-hidden="true">✳</div>
          <p>{{ text.footer }}</p>
          <div class="strip-stat"><strong>{{ rooms.length }}</strong><span>{{ text.bedrooms }}</span></div>
          <div class="strip-stat"><strong>{{ chalet.nombrePersonnesBase || '—' }}</strong><span>{{ text.people }}</span></div>
        </section>

        <section class="alpine-section">
          <div class="alpine-copy">
            <p class="eyebrow"><span class="eyebrow-line"></span>{{ text.aboutTitle }}</p>
            <h2>{{ text.alpineTitle }}</h2>
            <p class="alpine-lead">{{ text.alpineText }}</p>
            <div class="about-copy">
              <p>{{ text.aboutTextOne }}</p>
              <p>{{ text.aboutTextTwo }}</p>
            </div>
            <a
              class="button button-dark"
              href="https://www.google.com/maps/dir//45.9655278,6.4344167/@45.9655278,6.4344167,15z"
              target="_blank"
              rel="noopener noreferrer"
            >{{ text.directions }} <span aria-hidden="true">↗</span></a>
          </div>
          <div class="alpine-coordinate" aria-hidden="true">
            <span>45°57′55.9″N</span>
            <strong>6°26′03.9″E</strong>
            <span>LE GRAND-BORNAND · FRANCE</span>
          </div>
        </section>

        <section id="chambres" class="rooms-section section-wrap">
          <div class="section-heading">
            <div>
              <p class="eyebrow"><span class="eyebrow-line"></span>{{ text.roomsEyebrow }}</p>
              <h2>{{ text.roomsTitle }}</h2>
            </div>
            <p class="section-intro">{{ text.roomsIntro }}</p>
          </div>

          <div v-if="rooms.length" class="room-grid">
            <article v-for="(room, index) in rooms" :key="room.numero ?? room.nom" class="room-card">
              <div class="room-photo-wrap">
                <button
                  v-if="photoUrl(room.photos?.[0])"
                  class="gallery-image-button"
                  type="button"
                  :aria-label="`${text.openGallery}: ${room.nom}`"
                  @click="openGallery(room.photos, 0, room.nom)"
                >
                  <img class="room-photo" :src="photoUrl(room.photos[0], true)" :alt="room.nom" loading="lazy" />
                </button>
                <div v-else class="room-photo room-photo-placeholder" :class="`placeholder-${index % 2}`">
                  <span>{{ String(index + 1).padStart(2, '0') }} / ARAVIS</span>
                  <svg viewBox="0 0 600 380" preserveAspectRatio="xMidYMid slice" aria-hidden="true">
                    <path d="m-10 290 130-160 90 107 115-173 93 141 84-69 110 154v100H-10Z" />
                  </svg>
                </div>
                <button
                  v-if="validPhotos(room.photos).length > 1"
                  class="gallery-count room-gallery-count"
                  type="button"
                  :aria-label="`${text.openGallery}: ${room.nom}, ${validPhotos(room.photos).length} ${text.photoOf} ${validPhotos(room.photos).length}`"
                  @click="openGallery(room.photos, 0, room.nom)"
                >▧ {{ validPhotos(room.photos).length }} {{ language === 'fr' ? 'photos' : 'photos' }}</button>
                <span class="room-number">{{ String(index + 1).padStart(2, '0') }}</span>
              </div>

              <div class="room-content">
                <div class="room-title-row">
                  <div>
                    <h3>{{ room.nom }}</h3>
                    <p class="room-capacity">{{ text.peopleUpTo }} {{ room.nombrePersonnesMax || room.nombrePersonnes || '—' }} {{ text.people }}</p>
                  </div>
                  <div v-if="room.cout != null || room.nombrePersonnes != null" class="room-price">
                    <div v-if="room.cout != null" class="room-price-amount">
                      <strong>{{ formatPrice(room.cout) }}</strong><span>{{ text.perNight }}</span>
                    </div>
                    <span v-if="room.nombrePersonnes != null" class="room-price-capacity">
                      {{ text.forPeople }} {{ room.nombrePersonnes }} {{ text.people }}
                    </span>
                  </div>
                </div>

                <p v-if="localized(room.description) || room.pmr" class="room-description">
                  <span v-if="room.pmr" class="room-accessible">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
                      <circle cx="7.5" cy="17.5" r="4.5" />
                      <circle cx="14" cy="4" r="1.5" />
                      <path d="M12 8v5l4 2h3m-7-7 3 3h3m-8-3 3-2" />
                    </svg>
                    {{ text.accessible }}
                  </span>
                  {{ localized(room.description) }}
                </p>

                <div class="room-features">
                  <span v-if="room.terrasse">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M3 10h18L19 4H5l-2 6Zm0 0v2a3 3 0 0 0 6 0 3 3 0 0 0 6 0 3 3 0 0 0 6 0v-2M5 15v5m14-5v5M3 20h18" /></svg>
                    {{ text.terrace }}
                  </span>
                  <span v-if="room.balcon">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M4 20V9h16v11M3 20h18M8 9v11m8-11v11M4 13h16M8 4h8l2 5H6l2-5Z" /></svg>
                    {{ text.balcony }}
                  </span>
                  <span v-if="room.salleDeBain">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M4 11h16v3a5 5 0 0 1-5 5H9a5 5 0 0 1-5-5v-3Zm2 8-1 2m13-2 1 2M7 11V6a2 2 0 0 1 4 0v1" /></svg>
                    {{ room.salleDeBain }} {{ text.bathrooms }}
                  </span>
                  <span v-if="room.douche">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M4 11a8 8 0 0 1 16 0M3 11h18M6 15v1m4-1v1m4-1v1m4-1v1M6 19v1m4-1v1m4-1v1m4-1v1" /></svg>
                    {{ text.shower }}
                  </span>
                  <span v-if="room.baignoire">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M3 12h18v3a5 5 0 0 1-5 5H8a5 5 0 0 1-5-5v-3Zm3 8-1 2m13-2 1 2M6 12V6a2 2 0 0 1 4 0v1" /></svg>
                    {{ text.bath }}
                  </span>
                  <span v-if="room.wc">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M7 4h10v5H7zM5 9h14l-2 7a4 4 0 0 1-4 3h-2a4 4 0 0 1-4-3L5 9Zm5 10v2m4-2v2" /></svg>
                    {{ room.wc }} {{ text.toilets }}
                  </span>
                  <span v-if="room.tv">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><rect x="3" y="5" width="18" height="13" rx="1" /><path d="m8 22 4-4 4 4M9 2l3 3 3-3" /></svg>
                    {{ text.tv }}
                  </span>
                  <span v-if="room.wifi">
                    <svg viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M2 8a15 15 0 0 1 20 0M5 12a10 10 0 0 1 14 0m-11 4a5 5 0 0 1 8 0m-5 4h2" /></svg>
                    {{ text.wifi }}
                  </span>
                </div>

                <div v-if="room.lits?.length" class="beds-detail">
                  <span class="detail-label">{{ text.beds }}</span>
                  <div class="bed-grid">
                    <div v-for="(bed, bedIndex) in room.lits" :key="bedIndex" class="bed-card">
                      <div class="bed-count">
                        <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
                          <path d="M3 19V8m0 7h18v4M3 11h5a3 3 0 0 1 3 3v1m0-4h5a3 3 0 0 1 3 3v1M3 19v2m18-2v2" />
                        </svg>
                        <strong>{{ bed.nombre }}</strong>
                      </div>
                      <span class="bed-type">{{ bed.lit?.classe || '—' }}</span>
                      <span class="bed-width">
                        <strong>{{ bed.lit?.largeur != null && Number.isFinite(Number(bed.lit.largeur)) ? bed.lit.largeur : '—' }}</strong>
                        <small>cm</small>
                      </span>
                    </div>
                  </div>
                </div>
                <div v-if="room.vues?.length" class="room-detail">
                  <span class="detail-label">{{ text.views }}</span><span>{{ room.vues.map(localized).filter(Boolean).join(' · ') }}</span>
                </div>
                <div v-if="room.plusNuitMinimum && nonEmptyConditions(room.plusNuit).length" class="night-rate-rules">
                  <p class="night-rate-intro">
                    {{ text.baseRateForFirst }} {{ room.plusNuitMinimum }} {{ text.firstNights }}.
                  </p>
                  <p class="detail-label">{{ text.afterFirstNights }}</p>
                  <div v-for="(rate, rateIndex) in nonEmptyConditions(room.plusNuit)" :key="rateIndex" class="extra-row night-rate-row">
                    <span>{{ localized(rate.condition) }}<template v-if="rate.parPersonne"> · {{ text.perPerson }}</template></span>
                    <span>+{{ formatPrice(rate.cout) }}<template v-if="rate.par"> / {{ periodLabel(rate.par) }}</template><template v-else> / {{ text.nightRate }}</template></span>
                  </div>
                </div>

                <div v-if="nonEmptyConditions(room.prestationsPayantes).length" class="extras">
                  <p class="detail-label">{{ text.optional }}</p>
                  <div v-for="(extra, extraIndex) in nonEmptyConditions(room.prestationsPayantes)" :key="extraIndex" class="extra-row">
                    <span>{{ localized(extra.condition) }}</span>
                    <span>
                      {{ formatPrice(extra.cout) }}<template v-if="extra.par"> / {{ periodLabel(extra.par) }}</template>
                      <template v-if="extra.parPersonne"> · {{ text.perPerson }}</template>
                    </span>
                  </div>
                </div>

                <div v-if="room.animaux?.length || room.animauxAutorises" class="room-pets">
                  <span>{{ text.pets }}</span>
                  <span v-if="room.animauxAutorises">{{ text.permitted }}</span>
                  <span v-else>{{ text.notPermitted }}</span>
                </div>
              </div>
            </article>
          </div>
          <p v-else class="empty-note">{{ text.roomsIntro }}</p>
        </section>

        <section class="village-section">
          <div class="village-gallery">
            <img
              :src="villageSeason === 'winter' ? '/images/village-hiver-nuit-hd.jpg' : '/images/village-ete-jour-2-hd.jpg'"
              :alt="villageSeason === 'winter' ? text.villageWinterAlt : text.villageSummerAlt"
              loading="lazy"
            />
            <button
              class="village-season-toggle"
              type="button"
              @click="villageSeason = villageSeason === 'winter' ? 'summer' : 'winter'"
            >
              <span class="season-icon" aria-hidden="true">{{ villageSeason === 'winter' ? '☀' : '❄' }}</span>
              {{ villageSeason === 'winter' ? text.switchToSummer : text.switchToWinter }}
            </button>
          </div>
          <div class="village-copy">
            <p class="eyebrow"><span class="eyebrow-line"></span>{{ language === 'fr' ? 'À proximité' : 'Nearby' }}</p>
            <h2>{{ text.villageTitle }}</h2>
            <p>{{ text.villageTextOne }}</p>
            <p>{{ text.villageTextTwo }}</p>
            <a
              class="button button-light"
              href="https://www.legrandbornand.com/"
              target="_blank"
              rel="noopener noreferrer"
            >{{ text.discoverVillage }} <span aria-hidden="true">↗</span></a>
          </div>
        </section>

        <section class="bird-section">
          <div class="bird-copy">
            <p class="eyebrow"><span class="eyebrow-line"></span>{{ language === 'fr' ? 'La faune des Aravis' : 'Wildlife of the Aravis' }}</p>
            <h2>{{ text.birdTitle }}</h2>
            <p>{{ text.birdText }}</p>
          </div>
          <img class="bird-image" src="/images/gypaete-barbu.jpg" :alt="text.birdAlt" loading="lazy" />
        </section>

        <section v-if="chalet.prestations?.length || chalet.periodes?.length || chalet.prestationsPayantes?.length || chalet.animaux?.length" class="details-band">
          <div class="details-inner">
            <div v-if="chalet.prestations?.length" class="detail-column">
              <p class="eyebrow"><span class="eyebrow-line"></span>{{ text.included }}</p>
              <h2>{{ language === 'fr' ? 'Le plaisir des choses simples.' : 'The simple pleasures.' }}</h2>
              <ul class="simple-list">
                <li v-for="(service, index) in chalet.prestations" :key="index"><span>✳</span>{{ localized(service) }}</li>
              </ul>
            </div>

            <div v-if="chalet.periodes?.length" class="detail-column periods-column">
              <p class="eyebrow"><span class="eyebrow-line"></span>{{ text.availability }}</p>
              <h2>{{ text.availabilityIntro }}</h2>
              <div v-for="(period, index) in chalet.periodes" :key="index" class="period-row">
                <span>{{ formattedDate(period.dateDebut) }} — {{ formattedDate(period.dateFin) }}</span>
                <strong>{{ formatPrice(period.cout) }} <small>/ {{ periodLabel(period.par) }}</small></strong>
              </div>
            </div>

            <div v-if="chalet.prestationsPayantes?.length || chalet.animaux?.length" class="detail-column">
              <p class="eyebrow"><span class="eyebrow-line"></span>{{ text.optional }}</p>
              <h2>{{ text.pets }}</h2>
              <div v-if="chalet.animaux?.length" class="simple-list">
                <div v-for="(pet, index) in nonEmptyConditions(chalet.animaux)" :key="index" class="extra-row">
                  <span>{{ localized(pet.condition) }}</span><span>{{ formatPrice(pet.cout) }}<template v-if="pet.par"> / {{ periodLabel(pet.par) }}</template></span>
                </div>
              </div>
              <div v-if="nonEmptyConditions(chalet.prestationsPayantes).length" class="simple-list paid-list">
                <div v-for="(extra, index) in nonEmptyConditions(chalet.prestationsPayantes)" :key="index" class="extra-row">
                  <span>{{ localized(extra.condition) }}</span>
                  <span>{{ formatPrice(extra.cout) }}<template v-if="extra.par"> / {{ periodLabel(extra.par) }}</template><template v-if="extra.parPersonne"> · {{ text.perPerson }}</template></span>
                </div>
              </div>
            </div>
          </div>
        </section>

        <footer id="contact" class="contact-section">
          <div class="contact-heading">
            <p class="eyebrow"><span class="eyebrow-line"></span>{{ text.contactEyebrow }}</p>
            <h2>{{ text.contactTitle }}</h2>
            <p>{{ text.contactText }}</p>
            <a
              v-for="(link, index) in info.reservationLinks ?? []"
              :key="index"
              v-show="safeUrl(link)"
              class="button button-light"
              :href="safeUrl(link)"
              target="_blank"
              rel="noopener noreferrer"
            >
              {{ text.book }} <span aria-hidden="true">↗</span>
            </a>
          </div>

          <div class="contact-details">
            <div class="contact-item">
              <span class="contact-label">{{ text.address }}</span>
              <address>{{ info.postal }}</address>
            </div>
            <div v-if="info.email && safeUrl(`mailto:${info.email}`)" class="contact-item">
              <span class="contact-label">{{ text.email }}</span>
              <a :href="safeUrl(`mailto:${info.email}`)">{{ info.email }}</a>
            </div>
            <div v-if="info.phoneNumbers?.length" class="contact-item">
              <span class="contact-label">{{ text.call }}</span>
              <a v-for="(phone, index) in info.phoneNumbers" :key="index" :href="safeUrl(`tel:${phone.replaceAll(' ', '')}`)">{{ phone }}</a>
            </div>
            <a
              v-if="safeUrl(info.socialNetworks?.facebook)"
              class="social-link"
              :href="safeUrl(info.socialNetworks.facebook)"
              target="_blank"
              rel="noopener noreferrer"
            >Facebook <span aria-hidden="true">↗</span></a>
          </div>

          <div class="footer-bottom">
            <a class="footer-brand" href="#">{{ chalet.nom || 'Le Gypaète' }}</a>
            <span>{{ text.footer }}</span>
            <a href="#chalet" class="back-to-top">↑ <span>{{ language === 'fr' ? 'Haut de page' : 'Back to top' }}</span></a>
          </div>
          <div class="creator-mark">
            <img src="/images/lupogryph.svg" alt="Lupogryph" loading="lazy" />
          </div>
        </footer>
      </template>
    </main>

    <div v-if="gallery" class="gallery-lightbox" role="dialog" aria-modal="true" :aria-label="gallery.title" @click.self="closeGallery">
      <button class="gallery-close" type="button" :aria-label="text.closeGallery" @click="closeGallery">×</button>
      <button
        v-if="gallery.photos.length > 1"
        class="gallery-arrow gallery-previous"
        type="button"
        :aria-label="text.previousPhoto"
        @click="moveGallery(-1)"
      >‹</button>
      <figure class="gallery-figure">
        <img :src="photoUrl(gallery.photos[gallery.index])" :alt="gallery.title" />
        <figcaption>
          <span>{{ gallery.title }}</span>
          <span>{{ gallery.index + 1 }} {{ text.photoOf }} {{ gallery.photos.length }}</span>
        </figcaption>
      </figure>
      <button
        v-if="gallery.photos.length > 1"
        class="gallery-arrow gallery-next"
        type="button"
        :aria-label="text.nextPhoto"
        @click="moveGallery(1)"
      >›</button>
    </div>
  </div>
</template>
