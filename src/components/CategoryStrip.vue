<script setup>
/**
 * CategoryStrip.vue
 * Shop-style "Categories" row on the home page with links to each page.
 */

const categories = [
  { num: '01', jp: '私について', label: 'About', to: '/about' },
  { num: '02', jp: '作品', label: 'Portfolio', to: '/portfolio' },
  { num: '03', jp: 'お問い合わせ', label: 'Contact', to: '/about' },
]
</script>

<template>
  <section class="categories" aria-labelledby="categories-heading">
    <h2 id="categories-heading" class="categories-title">
      Categories <span class="categories-title-jp" lang="ja">カテゴリー</span>
    </h2>

    <ul class="category-grid">
      <li v-for="cat in categories" :key="cat.num">
        <RouterLink :to="cat.to" class="category">
          <span class="category-text">
            <span class="category-meta">
              {{ cat.num }} · <span lang="ja">{{ cat.jp }}</span>
            </span>
            <span class="category-name">{{ cat.label }}</span>
          </span>
          <span class="category-arrow" aria-hidden="true">→</span>
        </RouterLink>
      </li>
    </ul>
  </section>
</template>

<style scoped>
.categories {
  display: flex;
  flex-direction: column;
  gap: 1.125rem;
  padding: 2.5rem 2rem 3.5rem;
}

.categories-title {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: 0.5rem 0.875rem;
  margin: 0;
  font-family: var(--font-display);
  font-size: 1.4rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
}

.categories-title-jp {
  font-family: var(--font-jp);
  font-size: 0.875rem;
  font-weight: 700;
  letter-spacing: 0.1em;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  margin: 0;
  padding: 0;
  list-style: none;
  border-top: 1px solid var(--color-ink);
  border-left: 1px solid var(--color-ink);
  background: var(--color-surface);
}

.category-grid li {
  border-right: 1px solid var(--color-ink);
  border-bottom: 1px solid var(--color-ink);
}

.category {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  height: 100%;
  padding: 1.5rem;
  transition: background 0.2s ease;
}

.category:hover,
.category:focus-visible {
  background: var(--color-accent);
}

.category-text {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.category-meta {
  font-family: var(--font-jp);
  font-size: 0.8rem;
  font-weight: 800;
  color: var(--color-ink-soft);
}

.category:hover .category-meta,
.category:focus-visible .category-meta {
  color: var(--color-ink);
}

.category-name {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: 1.5rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.category-arrow {
  font-family: var(--font-display);
  font-size: 1.5rem;
  transition: transform 0.2s ease;
}

.category:hover .category-arrow,
.category:focus-visible .category-arrow {
  transform: translateX(6px);
}

@media (max-width: 600px) {
  .categories {
    padding: 2rem 1rem 2.5rem;
  }
}

@media (prefers-reduced-motion: reduce) {
  .category,
  .category-arrow {
    transition: none;
  }
}
</style>
