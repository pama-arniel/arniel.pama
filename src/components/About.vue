<template>
  <div v-scroll-animation>
    <SkillsPopup :showModal="showModal" @close-modal="showModal = false" />

    <section id="about-section" class="body-font overflow-hidden">
      <div class="font-inter container px-5 py-10 mx-auto">
        <div class="lg:w-4/5 mx-auto flex flex-wrap">
          <img
            alt="3D about section image"
            class="lg:w-1/2 w-full object-cover object-center about-image"
            src="@/assets/about/about_hero.png"
            v-scroll-animation="'fade-in-left'"
          />
          <div class="lg:w-1/2 w-full lg:pl-10 lg:py-6 mt-6 lg:mt-0">
            <h2 class="text-sm title-font text-gray-500 tracking-widest">
              ABOUT ME
            </h2>
            <h1 class="text-4xl title-font mb-8 purple-header">
              I am a well-rounded developer.
            </h1>
            <div
              v-for="description in descriptions"
              :key="description"
              class="leading-relaxed mb-6"
            >
              <div class="float-left">
                <span class="sm:text-2xl text-sm">⚙️ </span>
              </div>
              <div>
                <span
                  class="font-medium text-white my-6 pl-2 align-middle flex"
                >
                  {{ description }}
                </span>
              </div>
            </div>
            <div
              class="flex mt-4 items-center pb-5 border-b-2 border-gray-100 mb-5"
            >
              <div class="flex">
                <Button
                  :text="'See Skills'"
                  @button-clicked="showModal = true"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- the experience part -->
      <Experience />
    </section>
  </div>
</template>

<script>
import SkillsPopup from "./SkillsPopup.vue";
import Experience from "./Experience.vue";
import Button from "./Button.vue";

export default {
  name: "About",
  components: {
    SkillsPopup,
    Experience,
    Button,
  },
  data() {
    return {
      showModal: false,
      descriptions: [
        "I help clients develop their software ideas from concept to reality with an added touch of creativity and a spark of joy to my outputs.",
        "I work well with a team, enjoy exchanging ideas and collaboration, and meet targets with high-quality work.",
        "I can quickly master new software packages and hardware technologies. I love to learn new skills and explore something novel.",
      ],
    };
  },
};
</script>

<style scoped>
.fade-in-left {
  animation: fade-in-left 2s ease;
}

@keyframes fade-in-left {
  0% {
    opacity: 0;
    transform: translateX(-20px);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}

.about-image:hover {
  transform: scale(1.1);
}
</style>
