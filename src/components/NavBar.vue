<script setup>
/**
 * Navbar.vue
 * Navigation bar setup
 */


const links = [
    { label: 'Home', jp: 'ホーム', to: '/' },
    { label: 'About', jp: '私について', to: '/about' },
    { label: 'Portfolio', jp: '作品', to: '/portfolio' },
]
</script>

<template>
    <header class="site-header">
        <!-- Logo -->
        <RouterLink to="/" class="brand">
            <span class="brand-stamp" aria-hidden="true">店</span>
            <span class="brand-text">
                <span class="brand-name">Steven G.</span>
                <span class="brand-sub">
                    Software Dev Shop <span lang="ja">スティーブンの店</span>
                </span>
            </span>
        </RouterLink>


        <!-- Main Navigation Pill -->
        <nav class="nav-pill" aria-label="Main navigation">
            <RouterLink
                v-for="link in links"
                :key="link.label"
                :to="link.to"
                class="nav-link"
            >
                <span class="nav-en">{{ link.label }}</span>
                <span class="nav-jp" lang="ja">{{ link.jp }}</span>
            </RouterLink>
        </nav>
    </header>
</template>

<style scoped>

/* Site Layout */
.site-header {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: space-between;
    gap: 1rem 2rem;
    padding: 1rem 2rem;
}

/* Stamp Logo */
.brand {
    display: flex;
    align-items: center;
    gap: 0.875rem;
}


.brand-stamp {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    width: 3.5rem;
    height: 3.5rem;
    border: 2px solid var(--color-ink);
    border-radius: 50%;
    background: var(--color-accent);
    font-family: var(--font-jp);
    font-weight: 800;
    font-size: 1.6rem;
}

.brand-text {
    display: flex;
    flex-direction: column;
    gap: 0.125rem;
}

.brand-name {
    font-family: var(--font-display);
    font-weight: 700;
    font-size: 1.6rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
}

.brand-sub {
    font-family: var(--font-jp);
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
}

/* Navigation Pill */
.nav-pill {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 0.5rem 2.5rem;
    padding: 0.875rem 2.5rem;
    border-radius: 999px;
    background: var(--color-surface);
}

.nav-link {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.125rem;
    padding: 0.25rem 0;
}

.nav-en {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: 1.25rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  text-underline-offset: 6px;
}

.nav-jp {
  font-family: var(--font-jp);
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--color-ink-soft);
}


.nav-link:hover .nav-en,
.nav-link:focus-visible .nav-en,
.nav-link[aria-current='page'] .nav-en {
    text-decoration: underline;
    text-decoration-color: var(--color-accent);
    text-decoration-thickness: 3px;
}

.nav-link[aria-current='page'] .nav-jp {
  color: var(--color-accent-deep);
}

/* MOBILE  */

@media (max-width: 700px) {
    .site-header{
        flex-direction: column;
        justify-content: center;
    }

    .nav-pill {
        padding: 0.75rem 1.5rem;
        gap: 0.5rem 1.25rem;
    }

    .nav-en {
        font-size: 1rem;
        letter-spacing: 0.1em;
    }
}
</style>