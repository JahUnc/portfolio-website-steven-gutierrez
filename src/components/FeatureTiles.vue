<script setup>

import highlightsImg from '../assets/flowers.jpg'
import profileImg from '../assets/portrait.jpg'

const tiles = [
    {
        label: 'Highlights',
        href: '#highlights',
        src: highlightsImg,
        alt: 'Pink and white flowers',
        position: 'center',
    },
    {
        label: 'Contact',
        href: '#contact',
        src: profileImg,
        alt: 'Portrait of Steven Gutierrez',
        position: 'top'
    },
]
</script>

<template>
    <aside class="tiles">
        <a
            v-for="tile in tiles"
            :key="tile.label"
            :href="tile.href"
            class="tile"
        >
            <img    
                :src="tile.src"
                :alt="tile.alt"
                :style="{ objectPosition: tile.position }"
            />
            <span class="tile-label">{{ tile.label }}</span>
        </a>
    </aside>
</template>


<style scoped>
.tiles {
  display: grid;
  grid-template-rows: 1fr 1.5fr;
  border-left: 1px solid var(--color-ink);
}

.tile {
  position: relative;
  display: block;
  overflow: hidden;
  min-height: 260px;
}

.tile + .tile {
  border-top: 1px solid var(--color-ink);
}

.tile img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.tile:hover img,
.tile:focus-visible img {
  transform: scale(1.04);
}

.tile-label {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 1.5rem 1rem 0.75rem;
  font-family: var(--font-display);
  font-weight: 700;
  font-size: clamp(1.75rem, 4vw, 3.5rem);
  letter-spacing: 0.15em;
  text-transform: uppercase;
  text-align: center;
  color: #ffffff;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.55), transparent);
}

@media (max-width: 800px) {
  .tiles {
    grid-template-rows: auto;
    border-left: none;
    border-top: 1px solid var(--color-ink);
  }

  .tile {
    min-height: 320px;
  }
}
</style>