<script setup>
/**
 * HomeIntro.vue
 * left column of home page
 */
const fullName = 'Steven Gutierrez'

const intro = 
    'My name is Steven and I am an aspiring software developer. I am in my second ' +
    'year of college and currently interning as an IT Digital Analyst. This site ' +
    'is where I keep sharpening my development skills and showcase my past, ' +
    'current, and future projects.'

const career = 
    'My goal is to build a career in software development, with a growing interest ' +
    'in data science. So far I have worked with Python, Java, C#, SQL, and HTML/CSS, ' +
    'and I am always looking for the next thing to learn. If you need to contact me just click on my photo.'
</script>

<template>
    <section class="intro" aria-labelledby="home-heading">
        <h1 id="home-heading" class="heading">Welcome..</h1>
        <p class="name">I'm {{ fullName }}</p>

        <div class="notebook">
            <div class=holes aria-hidden="true">
                <span class="hole"></span>
                <span class="hole"></span>
                <span class="hole"></span>
            </div>

            <div class="lines">
                <p>Hello,</p>
                <p>{{ intro  }}</p>
                <p>{{ career }}</p>
            </div>
        </div>
    </section>
</template>

<style scoped>
.intro {
    padding: 2rem 2.5rem 3rem 1.5rem;
}

.heading {
    margin: 0;
    font-family: var(--font-display);
    font-weight: 700;
    font-size: clamp(2.5rem, 6vw, 5rem);
    letter-spacing: 0.3em;
    text-transform: uppercase;
}

.name {
    margin: 0.5rem 0 2rem;
    font-family: var(--font-display);
    font-size: clamp(1rem, 2.2vw, 1.5rem);
    letter-spacing: 0.15em;
    text-transform: uppercase;
}

.notebook {
    display: grid;
    grid-template-columns: 3.5rem 1fr;
}

.holes {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 0.2rem 0 calc(var(--line) * 0.25);
}

.hole {
    width: 2.75rem;
    height: 2.75rem;
    border: 2px solid var(--color-ink);
    border-radius: 50%;
}

.lines {
    font-family: var(--font-script);
    font-size: clamp(1.6rem, 2.6vw, 2.25rem);
    line-height: var(--line);
    padding-bottom: var(--line);
    background-image: repeating-linear-gradient(
        to bottom,
        transparent 0,
        transparent calc(var(--line) - 2px),
        var(--color-ink) calc(var(--line) - 2px),
        var(--color-ink) var(--line)
    );
}

.lines p {
    margin: 0;
}

@media (max-width: 600px) {
    .intro {
        padding: 1.5rem 1rem 2rem;
    }

    .heading {
        letter-spacing: 0.15em;
    }
    
    .notebook {
        grid-template-columns: 2.25rem 1fr;
    }

    .hole {
        width: 1.5rem;
        height: 1.5rem;
    }
}
</style>

