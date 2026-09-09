<script setup>
import { ref } from 'vue'

const selectedWeek = ref(2)

const reflections = [
  {
    week: 2,
    sections: [
      {
        type: 'takeaway',
        number: '01',
        title: 'Takeaway',
        content:
          'One of my takeaways from the reading is that tangible learning is just a tool. What really matters is how we integrate it into learning and how we design the learning process. Sometimes, we might think tangible learning is innovative, so adding it into learning will automatically make learning more effective. But the tool itself does not create meaningful learning. Its value really depends on educators’ creativity and how they use it in the learning process.',
      },
      {
        type: 'connection',
        number: '02',
        title: 'Connection to a real-world example',
        content:
          'This reading reminded me of LEGO Education SPIKE. Since I am a programmer and also a LEGO lover, the integration of these two really impressed me. I think this combination creates a new learning experience by making coding more visible. Learners can actually see how their code affects a physical object. LEGO also works well as a tangible learning element because it encourages students to learn through constructionism. They can learn through building, testing, and iterating on what they create.',
      },
      {
        type: 'question',
        number: '03',
        title: 'A burning question',
        content:
          'I believe tangible learning can create meaningful learning experiences for students, but I wonder if instruction can sometimes shape how students think a tangible tool should be used. For example, if we teach students to build a boat with LEGO, will they start to see those LEGO pieces mainly as tools for building a boat instead of exploring other possibilities? How can we design tangible learning activities that provide enough guidance while still encouraging students to think creatively and explore different ways of using the tools?',
      },
    ],
  },
  ...Array.from({ length: 8 }, (_, index) => ({ week: index + 3, sections: [] })),
]

const weeks = reflections.map(({ week }) => week)
const currentReflection = () => reflections.find(({ week }) => week === selectedWeek.value)
</script>

<template>
  <div class="page-shell">
    <header class="hero">
      <div class="hero-mark" aria-hidden="true"><span></span><span></span><span></span></div>
      <p class="eyebrow">Tangible learning · course journal</p>
      <h1>Weekly Reflections</h1>
      <p class="hero-subtitle">
        A collection of my thoughts, connections, and questions throughout the course.
      </p>
    </header>

    <main class="journal-content">
      <nav class="week-nav" aria-label="Choose a reflection week">
        <div class="week-nav-intro">
          <span class="nav-label">Browse entries</span>
          <span class="nav-hint">Select a week to read</span>
        </div>
        <div class="week-list">
          <button
            v-for="week in weeks"
            :key="week"
            class="week-button"
            :class="{ selected: selectedWeek === week }"
            type="button"
            :aria-current="selectedWeek === week ? 'page' : undefined"
            @click="selectedWeek = week"
          >
            <span>Week</span> {{ week }}
          </button>
        </div>
      </nav>

      <section class="reflection-area" :aria-live="currentReflection().sections.length ? 'polite' : 'off'">
        <div v-if="currentReflection().sections.length" class="reflection-card">
          <div class="card-heading">
            <div>
              <p class="card-kicker">Reflection entry</p>
              <h2>Week {{ selectedWeek }}</h2>
            </div>
            <span class="entry-status"><span class="status-dot"></span> In progress</span>
          </div>

          <div class="section-list">
            <article
              v-for="section in currentReflection().sections"
              :key="section.number"
              class="reflection-section"
              :class="`section-${section.type}`"
            >
              <div class="section-icon" aria-hidden="true">
                <span v-if="section.type === 'takeaway'">✦</span>
                <span v-else-if="section.type === 'connection'">↗</span>
                <span v-else>?</span>
              </div>
              <div class="section-copy">
                <h3><span>{{ section.number }}.</span> {{ section.title }}</h3>
                <p>{{ section.content }}</p>
              </div>
            </article>
          </div>
        </div>

        <div v-else class="empty-state">
          <div class="empty-icon" aria-hidden="true">✧</div>
          <p class="card-kicker">Week {{ selectedWeek }}</p>
          <h2>Reflection coming soon.</h2>
          <p>This week’s reflection has not been added yet.</p>
        </div>
      </section>
    </main>

    <footer class="page-footer">
      <span>Learning in progress</span>
      <span class="footer-line"></span>
      <span>2026</span>
    </footer>
  </div>
</template>

<style scoped>
:global(*) { box-sizing: border-box; }

:global(body) {
  background: #f4f8fc;
  color: #172d49;
  font-family: Georgia, 'Times New Roman', serif;
}

:global(button) { font: inherit; }

.page-shell {
  min-height: 100vh;
  overflow: hidden;
  background: linear-gradient(135deg, #f5f9fd 0%, #edf5fb 52%, #f8fbfe 100%);
}

.hero,
.journal-content,
.page-footer { width: min(1080px, calc(100% - 48px)); margin: 0 auto; }

.hero { position: relative; padding: 106px 0 68px; }
.hero-mark { display: flex; gap: 5px; margin-bottom: 28px; }
.hero-mark span { display: block; width: 36px; height: 6px; border-radius: 99px; background: #2a78bd; }
.hero-mark span:nth-child(2) { width: 17px; background: #8bc6e7; }
.hero-mark span:nth-child(3) { width: 7px; background: #d3e9f5; }
.eyebrow, .card-kicker, .nav-label { color: #2770ad; font: 700 0.72rem/1.2 Arial, sans-serif; letter-spacing: 0.13em; text-transform: uppercase; }
.hero h1 { max-width: 650px; margin: 14px 0 18px; color: #16395e; font-size: clamp(3rem, 7vw, 5.8rem); font-weight: 400; letter-spacing: -0.04em; line-height: 0.98; }
.hero-subtitle { max-width: 530px; margin: 0; color: #55718d; font-size: 1.14rem; line-height: 1.7; }

.journal-content { padding-bottom: 72px; }
.week-nav { display: flex; align-items: center; gap: 30px; padding: 15px 18px; border: 1px solid #d9e7f1; border-radius: 18px; background: rgba(255,255,255,0.78); box-shadow: 0 13px 35px rgba(44, 93, 131, 0.07); }
.week-nav-intro { min-width: 114px; }
.nav-label { display: block; margin-bottom: 5px; }
.nav-hint { color: #7891a8; font: 0.72rem/1.3 Arial, sans-serif; }
.week-list { display: flex; gap: 8px; overflow-x: auto; scrollbar-width: none; }
.week-list::-webkit-scrollbar { display: none; }
.week-button { flex: 0 0 auto; cursor: pointer; border: 1px solid transparent; border-radius: 10px; padding: 12px 14px; color: #54738f; background: #edf5fb; font: 600 0.8rem Arial, sans-serif; transition: transform 180ms ease, background 180ms ease, color 180ms ease, box-shadow 180ms ease; }
.week-button span { color: #8ba2b7; font-size: 0.67rem; font-weight: 400; }
.week-button:hover { transform: translateY(-2px); color: #1f639f; background: #e1f0f9; }
.week-button.selected { color: white; background: #216ba8; box-shadow: 0 6px 13px rgba(33, 107, 168, 0.23); }
.week-button.selected span { color: #b8daf0; }

.reflection-area { margin-top: 36px; }
.reflection-card, .empty-state { border: 1px solid #dce9f2; border-radius: 24px; background: rgba(255,255,255,0.9); box-shadow: 0 20px 55px rgba(35, 79, 113, 0.09); }
.reflection-card { padding: 48px 56px 54px; }
.card-heading { display: flex; align-items: flex-start; justify-content: space-between; gap: 20px; padding-bottom: 35px; border-bottom: 1px solid #e4eef5; }
.card-heading h2, .empty-state h2 { margin: 10px 0 0; color: #183d63; font-size: 2.25rem; font-weight: 400; }
.entry-status { display: flex; align-items: center; gap: 8px; margin-top: 5px; color: #7190a9; font: 0.72rem Arial, sans-serif; }
.status-dot { width: 7px; height: 7px; border-radius: 50%; background: #62b7a4; }
.section-list { display: grid; gap: 28px; padding-top: 34px; }
.reflection-section { display: grid; grid-template-columns: 42px minmax(0, 1fr); gap: 20px; padding: 2px 0 31px; border-bottom: 1px solid #e4eef5; }
.reflection-section:last-child { border-bottom: 0; padding-bottom: 0; }
.section-icon { display: grid; width: 40px; height: 40px; place-items: center; border-radius: 12px; color: #216ba8; background: #e7f3fa; font: 1.2rem Georgia, serif; }
.section-connection .section-icon { color: #5c8fae; background: #eef5f8; }
.section-question { margin: 0 -20px; padding: 25px 20px 8px; border: 0; border-radius: 16px; background: #edf7fc; }
.section-question .section-icon { color: white; background: #277bb8; }
.section-copy h3 { margin: 4px 0 13px; color: #244d73; font: 700 0.86rem/1.3 Arial, sans-serif; letter-spacing: 0.07em; text-transform: uppercase; }
.section-copy h3 span { color: #4c9bc8; }
.section-copy p { max-width: 735px; margin: 0; color: #4b657c; font-size: 1.05rem; line-height: 1.9; }

.empty-state { padding: 88px 24px; text-align: center; }
.empty-icon { display: grid; width: 56px; height: 56px; margin: 0 auto 23px; place-items: center; border: 1px solid #cce3f1; border-radius: 17px; color: #4c9bc8; background: #eff8fc; font-size: 1.7rem; }
.empty-state h2 { font-size: 2rem; }
.empty-state > p:last-child { margin: 14px 0 0; color: #7891a8; font-size: 1rem; }
.page-footer { display: flex; align-items: center; gap: 14px; padding: 0 0 38px; color: #7891a8; font: 0.7rem Arial, sans-serif; letter-spacing: 0.08em; text-transform: uppercase; }
.footer-line { width: 36px; height: 1px; background: #b7d6e8; }

@media (max-width: 700px) {
  .hero, .journal-content, .page-footer { width: min(100% - 32px, 540px); }
  .hero { padding: 60px 0 44px; }
  .hero h1 { font-size: clamp(2.8rem, 15vw, 4.5rem); }
  .week-nav { display: block; padding: 16px; }
  .week-nav-intro { margin-bottom: 14px; }
  .week-list { margin-right: -16px; padding-right: 16px; }
  .reflection-card { padding: 30px 22px 34px; }
  .card-heading h2 { font-size: 1.9rem; }
  .entry-status { font-size: 0; }
  .entry-status .status-dot { width: 9px; height: 9px; }
  .reflection-section { grid-template-columns: 34px minmax(0, 1fr); gap: 13px; }
  .section-icon { width: 34px; height: 34px; border-radius: 10px; font-size: 1rem; }
  .section-copy p { font-size: 0.98rem; line-height: 1.8; }
  .section-question { margin: 0 -8px; padding: 21px 8px 5px; }
}
</style>
