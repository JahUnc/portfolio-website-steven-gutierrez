<script setup>
/**
 * Announcement messages shown in the scrolling ticker.
 * @type {{ text: string, tone: 'light' | 'pink' | 'sky' }[]}
 */

const messages = [
    { text: "★ いらっしゃいませ ★ WELCOME TO STEVEN'S SHOP", tone: 'light' },
    { text: '★ 新着 NEW PROJECTS IN STOCK', tone: 'pink' },
    { text: '★ FREE SHIPPING ON ALL REPOS', tone: 'sky' },
]

/**
 * Messages repeated twice so each ticker group is wide enough to fill the screen.
 */
const loop = [...messages, ...messages]
</script>

<template>
    <div class="ticker" role="region" aria-label="Announcements">
        <div class="ticker-track">
            <ul class="ticker-group">
                <li v-for="(msg,index) in loop" :key="'a' + index" :class="['ticker-item', msg.tone]">
                    {{ msg.text }}            
                </li>
            </ul>

            <ul class="ticker-group" aria-hidden="true">
                <li v-for="(msg, index) in loop" :key="'b' + index" :class="['ticker-item', msg.tone]">
                    {{ msg.text }}
                </li>
            </ul>
        </div>
    </div>
</template>

<style scoped>
.ticker {
    overflow: hidden;
    background: var(--color-ink);
    color: var(--color-surface);
}

.ticker-track {
    display: flex;
    width: max-content;
    animation: ticker-scroll 40s linear infinite;
}

.ticker:hover .ticker-track {
    animation-play-state: paused;
}

.ticker-group {
    display: flex;
    margin: 0;
    padding: 0;
    list-style: none;
}

.ticker-item {
    padding: 0.65rem 3rem 0.65rem 0;
    font-family: var(--font-display);
    font-size: 0.9rem;
    letter-spacing: 0.15em;
    white-space: nowrap;
}

.ticker-item.pink{
    color: var(--color-accent);
}

.ticker-item.sky {
    color: var(--color-sky);
}

@keyframes ticker-scroll {
    to {
        transform: translateX(-50%);
    }
}

@media (prefers-reduced-motion: reduce) {
    .ticker-track {
        animation: none;
    }
}
</style>