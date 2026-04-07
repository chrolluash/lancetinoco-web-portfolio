<template>
  <section id="about" class="about">

    <div class="about__label reveal">
      <span class="u-label">About</span>
    </div>

    <!-- big statement -->
    <div class="about__statement reveal">
      <h2 class="about__heading">
        I build things<br>
        that <em>actually</em><br>
        work.
      </h2>
    </div>

    <!-- bio column -->
    <div class="about__body reveal">
      <p class="about__bio">
        I'm Lance — an Information Technology undergrad and an aspiring full-stack web developer
        . I build projects and systems for work, freelancing, and as a hobby. Reliable and easy 
        to work with a positive attitude, good communication skills, and cooperative approach in 
        any work environment. I take both simple and challenging tasks seriously, listen well to 
        instructions, and carry out my responsibilities with minimal supervision — always maintaining 
        respect and professionalism toward the people I work with.
      </p>

      <div class="about__pills">
        <span v-for="t in traits" :key="t" class="about__pill u-label">{{ t }}</span>
      </div>
    </div>

    <!-- timeline / milestones -->
    <div class="about__timeline">
      <div
        v-for="(item, i) in timeline"
        :key="i"
        class="about__entry reveal"
        :style="{ transitionDelay: (i * 0.1) + 's' }"
      >
        <span class="about__year u-label">{{ item.year }}</span>
        <div class="about__entry-body">
          <div class="about__entry-title">{{ item.title }}</div>
          <p class="about__entry-desc">{{ item.desc }}</p>
        </div>
      </div>
    </div>

  </section>
</template>

<script setup>
const traits = ['Web Development', 'Web UI/UX', 'IoT', 'Basic Tech Support', 'IT Helpdesk', 'Information Technology Undergrad']

const timeline = [
  {
    year: '2025 - 2026',
    title: 'Calofronia Clothing Inc. A Licensee of GUESS? USA — Technical, Helpdesk, & Web Dev Intern',
    desc: 'Did helpdesk, basic technical support, and web development at their IT department from October 2025 to March 2026.',
  },
  {
    year: '2023 - 2026',
    title: 'Pamantasang Lungsod ng Muntinlupa — Information Technology Undergrad',
    desc: '2nd yr to 4th yr of college, continued taking up Informaton Technology and my journey in the IT field. Expected to graduate in 2026.',
  },
  {
    year: '2022 - 2023',
    title: 'Lyceum of Alabang — Information Technology',
    desc: '1st yr of college, taking up Informaton Technology where I learned and honed my programming skills. Transferrred to Pamantasan Lungsod ng Muntinlupa after 1 year.',
  },
  {
    year: '2020 - 2022',
    title: 'Lyceum of Alabang — ICT Strand (Senior High School)',
    desc: 'Took up ICT Strand and learned basic html and css.',
  },
]
</script>

<style scoped>
.about {
  display: grid;
  grid-template-columns: 180px 1fr 1fr;
  grid-template-rows: auto auto;
  gap: 0;
  padding: 6rem 3rem;
  column-gap: 4rem;
  row-gap: 5rem;
}

.about__label {
  grid-column: 1;
  grid-row: 1;
  padding-top: 0.5rem;
}

.about__statement {
  grid-column: 2;
  grid-row: 1;
}
.about__heading {
  font-family: var(--font-display);
  font-size: clamp(2.8rem, 4.5vw, 5.5rem);
  font-weight: 300;
  line-height: 1.05;
  letter-spacing: -0.03em;
}
.about__heading em {
  font-style: italic;
}

.about__body {
  grid-column: 3;
  grid-row: 1;
  display: flex;
  flex-direction: column;
  gap: 1.4rem;
  padding-top: 0.4rem;
}
.about__bio {
  font-size: 0.78rem;
  line-height: 2;
  color: var(--gray);
}
.about__bio strong { color: var(--black); font-weight: 400; }
.about__pills {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 1rem;
}
.about__pill {
  border: 1px solid var(--lightgray);
  padding: 0.3rem 0.8rem;
  transition: background 0.2s, color 0.2s, border-color 0.2s;
}
.about__pill:hover {
  background: var(--black);
  color: var(--white);
  border-color: var(--black);
}

/* timeline */
.about__timeline {
  grid-column: 1 / -1;
  grid-row: 2;
  display: flex;
  flex-direction: column;
  gap: 0;
  border-top: 1px solid var(--lightgray);
}
.about__entry {
  display: grid;
  grid-template-columns: 180px 1fr;
  gap: 4rem;
  padding: 2rem 0;
  border-bottom: 1px solid var(--lightgray);
  align-items: start;
}
.about__year {
  padding-top: 0.3rem;
}
.about__entry-title {
  font-size: 0.72rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-bottom: 0.6rem;
}
.about__entry-desc {
  font-size: 0.72rem;
  line-height: 1.9;
  color: var(--gray);
  max-width: 560px;
}

@media (max-width: 900px) {
  .about {
    grid-template-columns: 1fr;
    padding: 4rem 1.5rem;
    row-gap: 3rem;
  }
  .about__label, .about__statement, .about__body { grid-column: 1; grid-row: auto; }
  .about__timeline { grid-column: 1; }
  .about__entry { grid-template-columns: 90px 1fr; gap: 1.5rem; }
}
</style>
