<script setup>

import { ref, computed, onMounted } from 'vue'

const GITHUB_USERNAME = 'JahUnc'

const API_URL = `https://api.github.com/users/${GITHUB_USERNAME}/repos?sort=updated&per_page=100`


const CACHE_TTL = 10 * 60 * 1000
const CACHE_KEY = `github-repos-${GITHUB_USERNAME}`


const repos = ref([])
const loading = ref(true)
const error = ref('')


const sortedRepos = computed(() =>
  [...repos.value].sort((a, b) => new Date(b.updated_at) - new Date(a.updated_at)),
)

function formatDate(iso) {
  return new Date(iso).toLocaleDateString('en-US', {
    month: 'short',
    day: 'numeric',
    year: 'numeric',
  })
}

function readCache() {
  try {
    const cached = JSON.parse(sessionStorage.getItem(CACHE_KEY))
    if (cached && Date.now() - cached.savedAt < CACHE_TTL) return cached.data
  } catch {
    /* ignore storage errors */
  }
  return null
}

function writeCache(data) {
  try {
    sessionStorage.setItem(CACHE_KEY, JSON.stringify({ savedAt: Date.now(), data }))
  } catch {
    /* ignore storage errors */
  }
}

async function loadRepos(force = false) {
  loading.value = true
  error.value = ''

  const cached = force ? null : readCache()
  if (cached) {
    repos.value = cached
    loading.value = false
    return
  }

  try {
    const response = await fetch(API_URL, {
      headers: { Accept: 'application/vnd.github+json' },
    })

    if (!response.ok) {
      if (
        (response.status === 403 || response.status === 429) &&
        response.headers.get('x-ratelimit-remaining') === '0'
      ) {
        throw new Error('GitHub is limiting requests right now. Please try again in a little while.')
      }
      throw new Error(`GitHub returned an error (status ${response.status}).`)
    }

    const data = await response.json()
    repos.value = data
    writeCache(data)
  } catch (err) {
    error.value =
      err instanceof TypeError
        ? 'Could not reach GitHub. Check your connection and try again.'
        : err.message
  } finally {
    loading.value = false
  }
}

onMounted(() => loadRepos())
</script>

<template>
  <main class="portfolio">
    <nav class="breadcrumb" aria-label="Breadcrumb">
      <RouterLink to="/">Home</RouterLink>
      <span aria-hidden="true">›</span>
      <span aria-current="page">Portfolio <span lang="ja">作品</span></span>
    </nav>

    <header class="page-head">
      <h1 class="page-title">Portfolio</h1>
      <span class="page-title-jp" lang="ja">作品一覧</span>
    </header>

    <p class="intro">
      Every project in the shop, pulled live from my
      <a
        :href="`https://github.com/${GITHUB_USERNAME}`"
        target="_blank"
        rel="noopener noreferrer"
        class="inline-link"
        >GitHub</a
      >. New repositories show up here automatically.
    </p>

    <!-- Loading -->
    <div v-if="loading" class="status" role="status" aria-live="polite">
      <span class="status-jp" lang="ja">読み込み中</span>
      <span>Loading projects<span class="dots" aria-hidden="true">...</span></span>
    </div>

    <!-- Error -->
    <div v-else-if="error" class="status status-error" role="alert">
      <span class="status-jp" lang="ja">エラー</span>
      <span>Sorry, the projects could not be loaded. {{ error }}</span>
      <button type="button" class="retry" @click="loadRepos(true)">Try again</button>
    </div>

    <!-- Empty -->
    <div v-else-if="sortedRepos.length === 0" class="status" role="status">
      <span class="status-jp" lang="ja">在庫なし</span>
      <span>No public projects yet. Check back soon.</span>
    </div>

    <!-- Repository cards -->
    <section v-else class="listing" aria-labelledby="listing-heading">
      <h2 id="listing-heading" class="section-title">
        Projects <span class="section-title-jp" lang="ja">商品</span>
        <span class="count">{{ sortedRepos.length }} items</span>
      </h2>

      <ul class="repo-grid">
        <li v-for="repo in sortedRepos" :key="repo.id" class="repo">
          <span class="repo-tag">
            {{ repo.language || 'Misc' }}
          </span>

          <h3 class="repo-name">
            <a :href="repo.html_url" target="_blank" rel="noopener noreferrer">
              {{ repo.name }}
              <span class="sr-only">(opens on GitHub in a new tab)</span>
            </a>
          </h3>

          <p class="repo-desc" :class="{ empty: !repo.description }">
            {{ repo.description || 'No description provided.' }}
          </p>

          <dl class="repo-meta">
            <div>
              <dt>Language <span lang="ja">言語</span></dt>
              <dd>{{ repo.language || 'Not specified' }}</dd>
            </div>
            <div>
              <dt>Updated <span lang="ja">更新日</span></dt>
              <dd>
                <time :datetime="repo.updated_at">{{ formatDate(repo.updated_at) }}</time>
              </dd>
            </div>
          </dl>

          <a
            :href="repo.html_url"
            target="_blank"
            rel="noopener noreferrer"
            class="repo-button"
            aria-hidden="true"
            tabindex="-1"
          >
            View on GitHub →
          </a>
        </li>
      </ul>
    </section>
  </main>
</template>

<style scoped>
.portfolio {
  border-top: 6px solid var(--color-ink);
  min-height: 60vh;
}

/* Breadcrumb (matches About page) */
.breadcrumb {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  padding: 0.875rem 2rem;
  border-bottom: 1px solid var(--color-ink);
  font-family: var(--font-jp);
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.breadcrumb a:hover,
.breadcrumb a:focus-visible {
  text-decoration: underline;
}

/* Page heading */
.page-head {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: 0.5rem 1.25rem;
  padding: 2.5rem 2rem 0;
}

.page-title {
  margin: 0;
  font-family: var(--font-display);
  font-weight: 700;
  font-size: clamp(2.25rem, 5vw, 4rem);
  letter-spacing: 0.2em;
  text-transform: uppercase;
}

.page-title-jp {
  font-family: var(--font-jp);
  font-size: 1.1rem;
  font-weight: 800;
  letter-spacing: 0.1em;
}

.intro {
  max-width: 60ch;
  margin: 0;
  padding: 1rem 2rem 2rem;
  font-family: var(--font-jp);
  font-size: 1.05rem;
  line-height: 1.6;
}

.inline-link {
  text-decoration: underline;
  text-decoration-color: var(--color-accent);
  text-decoration-thickness: 3px;
  text-underline-offset: 4px;
}

.inline-link:hover,
.inline-link:focus-visible {
  color: var(--color-accent-deep);
}

/* Loading, error, and empty messages */
.status {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.75rem 1rem;
  margin: 0 2rem 3.5rem;
  padding: 1.5rem 1.75rem;
  border: 1px solid var(--color-ink);
  background: var(--color-surface);
  font-family: var(--font-jp);
  font-size: 1.05rem;
  font-weight: 700;
}

.status-jp {
  padding: 0.2rem 0.625rem;
  border: 1px solid var(--color-ink);
  background: var(--color-sky);
  font-size: 0.8rem;
  font-weight: 800;
}

.status-error .status-jp {
  background: var(--color-accent);
}

.dots {
  display: inline-block;
  animation: blink 1s steps(2, start) infinite;
}

@keyframes blink {
  to {
    visibility: hidden;
  }
}

.retry {
  margin-left: auto;
  padding: 0.5rem 1.125rem;
  border: 2px solid var(--color-ink);
  border-radius: 999px;
  background: var(--color-accent);
  color: var(--color-ink);
  font-family: var(--font-display);
  font-size: 0.9rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  cursor: pointer;
}

.retry:hover,
.retry:focus-visible {
  background: var(--color-accent-deep);
  color: #ffffff;
}

/* Listing */
.listing {
  display: flex;
  flex-direction: column;
  gap: 1.125rem;
  padding: 2rem 2rem 3.5rem;
  border-top: 1px solid var(--color-ink);
}

.section-title {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: 0.5rem 0.75rem;
  margin: 0;
  font-family: var(--font-display);
  font-size: 1.25rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
}

.section-title-jp {
  font-family: var(--font-jp);
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 0.1em;
}

.count {
  margin-left: auto;
  font-family: var(--font-jp);
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  color: var(--color-ink-soft);
}

/* Card grid with shared borders, like the About skills grid */
.repo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  margin: 0;
  padding: 0;
  list-style: none;
  border-top: 1px solid var(--color-ink);
  border-left: 1px solid var(--color-ink);
}

.repo {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  padding: 1.5rem 1.375rem;
  border-right: 1px solid var(--color-ink);
  border-bottom: 1px solid var(--color-ink);
  background: var(--color-surface);
  transition: background 0.2s ease;
}

.repo:hover {
  background: #ffffff;
}

.repo-tag {
  align-self: flex-start;
  padding: 0.2rem 0.625rem;
  border: 1px solid var(--color-ink);
  background: var(--color-sky);
  font-family: var(--font-jp);
  font-size: 0.75rem;
  font-weight: 800;
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

.repo-name {
  margin: 0;
  font-family: var(--font-display);
  font-weight: 700;
  font-size: 1.25rem;
  letter-spacing: 0.06em;
  overflow-wrap: anywhere;
}

.repo-name a:hover,
.repo-name a:focus-visible {
  text-decoration: underline;
  text-decoration-color: var(--color-accent);
  text-decoration-thickness: 3px;
  text-underline-offset: 5px;
}

.repo-desc {
  flex-grow: 1;
  margin: 0;
  font-family: var(--font-jp);
  font-size: 0.95rem;
  line-height: 1.55;
}

.repo-desc.empty {
  color: var(--color-ink-soft);
  font-style: italic;
}

.repo-meta {
  display: grid;
  grid-template-columns: 1fr 1fr;
  margin: 0;
  border: 1px solid var(--color-ink);
  font-family: var(--font-jp);
  font-size: 0.85rem;
}

.repo-meta div {
  padding: 0.5rem 0.75rem;
}

.repo-meta div + div {
  border-left: 1px solid var(--color-ink);
}

.repo-meta dt {
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--color-ink-soft);
}

.repo-meta dd {
  margin: 0.125rem 0 0;
  font-weight: 700;
}

.repo-button {
  align-self: flex-start;
  padding: 0.4rem 1rem;
  border: 2px solid var(--color-ink);
  border-radius: 999px;
  background: var(--color-accent);
  font-family: var(--font-display);
  font-size: 0.8rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.repo-button:hover {
  background: var(--color-accent-deep);
  color: #ffffff;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Mobile */
@media (max-width: 600px) {
  .breadcrumb,
  .page-head,
  .intro,
  .listing {
    padding-left: 1rem;
    padding-right: 1rem;
  }

  .status {
    margin-left: 1rem;
    margin-right: 1rem;
  }

  .retry {
    margin-left: 0;
  }
}
</style>
