<script setup>
    /**
     * HomeView.Vue
     * Portfolio Main Page
     */

    import HomeIntro from '../components/HomeIntro.vue'
    import FeatureTiles from '../components/FeatureTiles.vue'
</script>

<template>
    <main id="home" class="home">
        <HomeIntro />
        <FeatureTiles />
    </main>
</template>

<style scoped>
.home {
    display: grid;
    grid-template-columns: 1.75fr 1fr;
    border-top: 6px solid var(--color-ink);
    min-height: calc(100vh - 140px);
}

@media (max-width: 800px) {
    .home {
        grid-template-columns: 1fr;
    }
}
</style>
