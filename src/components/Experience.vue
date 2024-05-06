<template>
  <div
    class="container text-white font-inter px-10 pt-10 pb-24 mx-auto"
    v-scroll-animation
  >
    <div class="flex flex-wrap -mx-4 -my-8">
      <div
        v-for="(experience, index) in experiences"
        :key="'experience-' + index"
        class="py-8 px-4 lg:w-1/2 scale-on-hover"
      >
        <div class="h-full flex items-start section-background">
          <div
            class="w-15 md:flex-shrink-0 md:flex md:flex-col text-left leading-none hidden"
          >
            <span class="pb-2 mb-2 border-b-2 border-gray-500">{{
              experience.startDate
            }}</span>
            <span class="title-font leading-none">{{
              experience.endDate
            }}</span>
          </div>
          <div class="flex-grow md:pl-6">
            <p class="leading-relaxed text-gray-600 md:hidden block">
              {{ experience.startDate }} - {{ experience.endDate }}
            </p>
            <h1 class="title-font text-3xl mb-2 purple-header align-top">
              {{ experience.position }}
            </h1>
            <h2 class="tracking-widest font-bold">{{ experience.company }}</h2>
            <p class="leading-relaxed mb-5 text-gray-500">
              {{ experience.description }}
            </p>
            <div
              v-for="(task, index) in experience.tasks"
              :key="'task-' + index"
              class="leading-relaxed mb-6"
            >
              <div class="float-left">
                <span class="sm:text-2xl text-sm">⚙️</span>
              </div>
              <div>
                <span class="text-white my-6 pl-2 align-middle flex">
                  {{ task }}
                </span>
              </div>
            </div>
            <a
              :href="experience.website"
              target="_blank"
              class="inline-flex items-center"
            >
              <img
                :alt="`${experience.position} experience`"
                :src="getPic(experience.logo)"
                class="w-12 h-12 rounded-full flex-shrink-0 object-cover object-center company-logo"
              />
              <span class="flex-grow flex flex-col pl-3">
                <span class="title-font text-indigo-500">{{
                  experience.website
                }}</span>
              </span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Experience",
  data() {
    return {
      experiences: [
        {
          startDate: "Jun 2023",
          endDate: "Present",
          position: "Full-Stack Developer - VueJS and Firebase",
          company: "Seikosoft",
          description: "Technologies: VueJS 3, Firebase",
          tasks: [
            "One of the lead developers for Seikosoft, with the aim to develop an ecosystem for a laundry business that involves multiple user types such as a driver, customer, manager, and laundry washboss",
            "Mainly use VueJS and Firebase for our technology stack",
          ],
          logo: "seikosoft.png",
          website: "https://www.seikosoft.com/",
        },
        {
          startDate: "Dec 2021",
          endDate: "Jun 2023",
          position: "Frontend Developer - VueJS, Quasar, TailwindCSS",
          company: "LUMEA",
          description:
            "Technologies: VueJS 3, Quasar, TailwindCSS, MirageJS, Vuex 4, Pinia, Cypress",
          tasks: [
            "Helped implement and optimize medical technology processes for hospitals and patients",
            "Implemented front-end logic and UI designs using VueJS, Quasar, and TailwindCSS",
            "Used Vue 3 concepts and practices such as the Composition API",
            "Used cutting-edge VueJS technologies like MirageJS for API mocking, Vuex 4 and Pinia for store management, and Cypress for E2E testing",
          ],
          logo: "lumea.png",
          website: "https://lumeadigital.com/",
        },
        {
          startDate: "Jun 2021",
          endDate: "Oct 2021",
          position: "Software Developer - JavaScript, C#, Flutter",
          company: "Dewmaker Design, Inc.",
          description:
            "Technologies: Flutter, Firebase, Android Jetpack Compose,  C#, ASP .Net, Javascript, jQuery, MSSQL, Azure DevOps, Adaptive Cards, Bot Framework SDK",
          tasks: [
            "Worked on both the front-end (creating templates, components, widgets, applying logic, etc.) and back-end (authentication, CRUD operations) for a video-conferencing app built using Flutter, Android Jetpack Compose, and Firebase",
            "Worked on both the front-end (creating UI, applying logic) using Javascript and back-end logic (CRUD operations, stored procedures, controllers, etc.) using C# and MSSQL for an employee management app",
            "Developed an MS Teams bot complementary to Employee Hub using C# and the Bot Framework SDK (v4), used Adaptive Cards (adaptivecards.io) for the UI, and deployed the bot on Microsoft Azure",
          ],
          logo: "dewmaker.png",
          website: "https://dewmaker.design/",
        },
        {
          startDate: "Oct 2020",
          endDate: "Jun 2021",
          position: "Frontend Developer - VueJS, Flutter, and Figma",
          company: "NUVENSIS / BEAPTECH Inc.",
          description:
            "Technologies: Flutter, VueJS, PostgreSQL, GraphQL, Axios, Quasar, Tailwind CSS, Node.js, Figma",
          tasks: [
            "Developed mobile apps using Flutter,  BLoC pattern (store management), Hive (for storage), and PostgreSQL and GraphQL (for backend)",
            "Developed web apps using Vue.js, Quasar, Tailwind CSS, Node.js, and PostgreSQL and GraphQL",
            "Designed UI using Figma and Dribbble",
            "Implemented code and logic for frontend",
          ],
          logo: "nuvensis.png",
          website: "http://nuvensis.ch/",
        },
        {
          startDate: "July 2019",
          endDate: "Aug 2020",
          position: "Full-Stack Developer - VueJS, Python and Django",
          company: "BPOSeats.com / Channel Info Tech",
          description:
            "Technologies: VueJS, Python Django, REST API, jQuery, HTML, CSS, SQL, PostgreSQL, Confluence, JIRA, Git, Scrum, Agile, AWS",
          tasks: [
            "Back-end development, front-end development, implementation of HRIS-related features",
            "Managing some small teams, code reviews, some research-related tasks, Agile Development, Scrum",
            "Bug fixing, operational feedback, email and SMS notifications, release notes",
            "Unit testing, code rehauls, back-end schema, implement celery tasks",
          ],
          logo: "bposeats.png",
          website: "https://bposeats.com/",
        },
        {
          startDate: "Jun 2018",
          endDate: "Aug 2018",
          position: "Full-Stack Web Developer (Intern)",
          company: "BPOSeats.com / Channel Info Tech",
          description:
            "Technologies: VueJS, Python Django, REST API, jQuery, HTML, CSS, MongoDB, OpenTok",
          tasks: [
            'Developed "GuessWhut!?", an online video platform for playing charades with friends',
            "Awarded as the TOP APP among the 5 other competitors in our internship",
            "Used Django as web framework; CSS, HTML, and Vue.js for the UI/UX; MongoDB for the database; OpenTok library for video communication",
          ],
          logo: "bposeats.png",
          website: "https://bposeats.com/",
        },
      ],
    };
  },
  methods: {
    getPic(logoFile) {
      let images = require.context("../assets/experience/", false, /\.png$/);
      return images("./" + logoFile);
    },
  },
};
</script>

<style scoped>
.company-logo {
  box-shadow: 0px 0px 5px #ffffff;
  padding: 7px;
}
</style>
