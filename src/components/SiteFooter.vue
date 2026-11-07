<script setup>
const year = new Date().getFullYear()
</script>

<template>
    <footer class="site-footer">
        <p>© {{ year }} Steven Gutierrez</p>
        <p>ありがとうございました · THANK YOU FOR VISITING</p>
    </footer>
</template>

<style scoped>
.site-footer {
    display:flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 0.75rem 1.5rem;
    padding: 1.25rem 2rem;
    background: var(--color-ink);
    color: var(--color-surface);
    font-family: var(--font-display);
    font-size: 0.8rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
}

.site-footer p {
    margin: 0;
}

@media (max-width: 600px) {
    .site-footer {
        flex-direction: column;
        align-items: center;
        text-align: center;
    }
}
</style>
