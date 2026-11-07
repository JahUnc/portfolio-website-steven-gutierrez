<script setup>
    /**
     * HomeView.Vue
     * Portfolio Main Page
     */

    import HomeIntro from '../components/HomeIntro.vue'
    import FeatureTiles from '../components/FeatureTiles.vue'
    import CategoryStrip from '../components/CategoryStrip.vue'
</script>

<template>
    <main id="home">
        <div class="home">
            <HomeIntro />
            <FeatureTiles />
        </div>
        <CategoryStrip />
    </main>
</template>

<style scoped>
.home {
    display: grid;
    grid-template-columns: 1.75fr 1fr;
    border-top: 6px solid var(--color-ink);
    border-bottom: 1px solid var(--color-ink);
    min-height: calc(100vh - 140px);
}

@media (max-width: 800px) {
    .home {
        grid-template-columns: 1fr;
    }
}
</style>
