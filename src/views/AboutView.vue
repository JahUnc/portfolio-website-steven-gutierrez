<script setup>
/**
 * AboutView.vue
 * About page (/about), styled as a shop owner profile:
 * portrait with stamp, store info table, skills, and a to-do list of goals.
 */
import portraitImg from '../assets/portrait.jpg'

const info = [
  { label: 'Name', jp: '名前', value: 'Steven Gutierrez' },
  { label: 'Education', jp: '学歴', value: "IT/CS associate's degree, second year" },
  {
    label: 'Work',
    jp: '仕事',
    value:
      'Full-time IT Digital Analyst intern. I help people troubleshoot their devices, and keep company devices running',
  },
  { label: 'Honors', jp: '受賞', value: "Dean's List, twice" },
  { label: 'Interests', jp: '興味', value: 'Software development and data science' },
]

const skills = [
  { name: 'Python', category: 'Language', inStock: true },
  { name: 'Java', category: 'Language', inStock: true },
  { name: 'C#', category: 'Language', inStock: true },
  { name: 'SQL', category: 'Database', inStock: true },
  { name: 'HTML/CSS', category: 'Web', inStock: true },
  { name: 'Vue', category: 'Framework', inStock: true },
  { name: 'Data Science', category: 'Next Level', inStock: false },
]

const todos = [
  { text: "Make the Dean's List", done: true },
  { text: 'Land an IT internship', done: true },
  { text: "Finish my IT/CS associate's degree", done: false },
  { text: "Start a bachelor's in data science", done: false },
  { text: 'Build a career in software development', done: false },
]
</script>

<template>
  <main class="about">
    <nav class="breadcrumb" aria-label="Breadcrumb">
      <RouterLink to="/">Home</RouterLink>
      <span aria-hidden="true">›</span>
      <span aria-current="page">About <span lang="ja">私について</span></span>
    </nav>

    <header class="page-head">
      <h1 class="page-title">DEVELOPER</h1>
      <span class="page-title-jp" lang="ja">プロフィール</span>
    </header>

    <div class="profile">
      <div class="portrait">
        <img :src="portraitImg" alt="Portrait of Steven Gutierrez" />
        <div class="stamp" aria-hidden="true">
          <span lang="ja">インターン</span>
          <span>IT Intern</span>
        </div>
      </div>

      <section class="info" aria-labelledby="info-heading">
        <h2 id="info-heading" class="section-title">
          Dev. Info <span class="section-title-jp" lang="ja">店舗情報</span>
        </h2>
        <table class="info-table">
          <tbody>
            <tr v-for="row in info" :key="row.label">
              <th scope="row">
                <span class="info-label">{{ row.label }}</span>
                <span class="info-jp" lang="ja">{{ row.jp }}</span>
              </th>
              <td>{{ row.value }}</td>
            </tr>
          </tbody>
        </table>
      </section>
    </div>

    <section class="skills" aria-labelledby="skills-heading">
      <h2 id="skills-heading" class="section-title">
        Skills <span class="section-title-jp" lang="ja">スキル</span>
      </h2>
      <ul class="skill-grid">
        <li
          v-for="skill in skills"
          :key="skill.name"
          :class="['skill', { soon: !skill.inStock }]"
        >
          <span class="skill-category">{{ skill.category }}</span>
          <span class="skill-name">{{ skill.name }}</span>
          <span class="skill-badge">
            <template v-if="skill.inStock"><span lang="ja">在庫あり</span> In Stock</template>
            <template v-else><span lang="ja">近日入荷</span> Coming Soon</template>
          </span>
        </li>
      </ul>
    </section>

    <section class="todo" aria-labelledby="todo-heading">
      <div class="todo-card">
        <h2 id="todo-heading" class="section-title">
          To Do List <span class="section-title-jp" lang="ja">やることリスト</span>
        </h2>
        <ul class="todo-list">
          <li v-for="item in todos" :key="item.text" :class="['todo-item', { done: item.done }]">
            <span class="todo-box" aria-hidden="true">{{ item.done ? '✓' : '' }}</span>
            <span class="sr-only">{{ item.done ? 'Done:' : 'To do:' }}</span>
            <span class="todo-text">{{ item.text }}</span>
          </li>
        </ul>
      </div>
    </section>
  </main>
</template>

<style scoped>
.about {
  border-top: 6px solid var(--color-ink);
}

/* Breadcrumb */
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

/* Shared section headings */
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

/* Portrait and store info */
.profile {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  padding: 1.75rem 2rem 2.5rem;
}

.portrait {
  position: relative;
  flex: 1 1 280px;
  max-width: 380px;
}

.portrait img {
  width: 100%;
  aspect-ratio: 4 / 5;
  object-fit: cover;
  object-position: top;
  border: 1px solid var(--color-ink);
  outline: 10px solid var(--color-surface);
  outline-offset: -11px;
}

/* Hanko-style stamp */
.stamp {
  position: absolute;
  top: -1.25rem;
  right: -1.25rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.15rem;
  width: 6.5rem;
  height: 6.5rem;
  border: 4px double var(--color-accent-deep);
  border-radius: 50%;
  background: var(--color-surface);
  color: var(--color-accent-deep);
  font-family: var(--font-jp);
  font-weight: 800;
  font-size: 1.1rem;
  line-height: 1;
  text-align: center;
  animation: wobble 3s ease-in-out infinite;
}

.stamp span:last-child {
  font-family: var(--font-display);
  font-size: 0.7rem;
  letter-spacing: 0.1em;
  margin-right: -0.1em;
  text-transform: uppercase;
  white-space: nowrap;
}

@keyframes wobble {
  0%,
  100% {
    transform: rotate(10deg);
  }
  50% {
    transform: rotate(4deg);
  }
}

.info {
  flex: 999 1 460px;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 0.875rem;
}

.info-table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid var(--color-ink);
  background: var(--color-surface);
  font-family: var(--font-jp);
  font-size: 1.05rem;
  line-height: 1.5;
}

.info-table tr + tr {
  border-top: 1px solid var(--color-ink);
}

.info-table th {
  width: 11rem;
  padding: 0.875rem 1.125rem;
  border-right: 1px solid var(--color-ink);
  background: var(--color-sky);
  text-align: left;
  vertical-align: top;
}

.info-table td {
  padding: 0.875rem 1.125rem;
}

.info-label {
  display: block;
  font-family: var(--font-display);
  font-size: 0.95rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.info-jp {
  font-size: 0.75rem;
  font-weight: 800;
}

/* Skills */
.skills {
  display: flex;
  flex-direction: column;
  gap: 1.125rem;
  padding: 2rem;
  border-top: 1px solid var(--color-ink);
}

.skill-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  margin: 0;
  padding: 0;
  list-style: none;
  border-top: 1px solid var(--color-ink);
  border-left: 1px solid var(--color-ink);
}

.skill {
  display: flex;
  flex-direction: column;
  gap: 0.625rem;
  padding: 1.25rem 1.125rem;
  border-right: 1px solid var(--color-ink);
  border-bottom: 1px solid var(--color-ink);
  background: var(--color-surface);
}

.skill.soon {
  background: #ececec;
}

.skill-category {
  font-family: var(--font-jp);
  font-size: 0.75rem;
  font-weight: 800;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--color-ink-soft);
}

.skill-name {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: 1.5rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.skill-badge {
  align-self: flex-start;
  padding: 0.2rem 0.625rem;
  border: 1px solid var(--color-ink);
  background: var(--color-sky);
  font-family: var(--font-jp);
  font-size: 0.75rem;
  font-weight: 800;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  white-space: nowrap;
}

.skill.soon .skill-badge {
  background: var(--color-accent);
  white-space: normal;
}

/* To do list */
.todo {
  padding: 0.5rem 2rem 3.5rem;
}

.todo-card {
  display: flex;
  flex-direction: column;
  gap: 0.375rem;
  padding: 1.5rem 1.75rem 0.5rem;
  border: 1px solid var(--color-ink);
  background: var(--color-surface);
}

.todo-list {
  --line: 3rem;
  margin: 0;
  padding: 0 0 var(--line);
  list-style: none;
  font-family: 'Indie Flower', cursive;
  font-size: 1.75rem;
  line-height: var(--line);
  background-image: repeating-linear-gradient(
    to bottom,
    transparent 0,
    transparent calc(var(--line) - 2px),
    var(--color-ink) calc(var(--line) - 2px),
    var(--color-ink) var(--line)
  );
}

.todo-item {
  display: flex;
  align-items: flex-start;
  gap: 0.875rem;
  min-height: var(--line);
}

.todo-box {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 1.5rem;
  height: 1.5rem;
  margin-top: calc((var(--line) - 1.5rem) / 2);
  border: 2px solid var(--color-ink);
  font-family: var(--font-display);
  font-size: 1.1rem;
  line-height: 1;
  color: var(--color-accent-deep);
}

.todo-item.done .todo-text {
  text-decoration: line-through;
  text-decoration-color: var(--color-accent-deep);
  text-decoration-thickness: 2px;
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
  .profile,
  .skills,
  .todo {
    padding-left: 1rem;
    padding-right: 1rem;
  }

  .stamp {
    right: -0.5rem;
  }

  .info-table th {
    width: 7.5rem;
  }

  .todo-list {
    font-size: 1.4rem;
  }
}

</style>
