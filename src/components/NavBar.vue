<script setup>
/**
 * Navbar.vue
 * Navigation bar setup
 */

const links = [
    { label: 'Home', href: '#home'},
    { label: 'Highlights', href: '#highlights' },
    { label: 'Contact', href: '#contact' },
]
</script>

<template>
    <header class="site-header">
        <nav class="nav-pill" aria-label="Main navigation">
            <a
                v-for="link in links"
                :key="link.label"
                :href="link.href"
                class="nav-link"
            >
                {{ link.label }}
            </a>
        </nav>
    </header>
</template>

<style scoped>
.site-header {
    display: flex;
    justify-content: center;
    padding: 0.75rem 1rem;
}

.nav-pill {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 0.5rem 2.5rem;
    background: var(--color-surface);
    border-radius: 999px;
    padding: 1.25rem 3.5rem;
}

.nav-link {
    font-family: var(--font-display);
    font-weight: 700;
    font-size: 1.5rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    text-underline-offset: 6px;
}

.nav-link:hover,
.nav-link:focus-visible {
    text-decoration: underline;
    text-decoration-color: var(--color-accent);
    text-decoration-thickness: 3px;
}

@media (max-width: 600px) {
    .nav-pill {
        padding: 0.9rem 1.5rem;
        gap: 0.5rem 1.25rem;
    }

    .nav-link {
        font-size: 1rem;
        letter-spacing: 0.1em;
    }
}
</style>