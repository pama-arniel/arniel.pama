<template>
  <div>
    <Particles id="tsparticles" :options="configData" />

    <div id="app">
      <Nav class="mb-6" />
      <Hero />
      <About />
      <Projects />
      <Articles />
      <Organizations />
      <Contact />
      <Footer />
    </div>
  </div>
</template>

<script>
import Nav from "./components/Nav.vue";
import Hero from "./components/Hero.vue";
import About from "./components/About.vue";
import Projects from "./components/Projects.vue";
import Articles from "./components/Articles.vue";
import Contact from "./components/Contact.vue";
import Organizations from "./components/Organizations.vue";
import Footer from "./components/Footer.vue";

import particlesConfig from "./assets/particles_config.json";

export default {
  name: "App",
  components: {
    Nav,
    Hero,
    About,
    Projects,
    Articles,
    Contact,
    Organizations,
    Footer,
  },
  data() {
    return {
      configData: particlesConfig,
    };
  },
};
</script>

<style scoped>
canvas {
  display: block;
}
/* ---- tsparticles container ---- */
#tsparticles {
  position: fixed;
  width: 100%;
  height: calc(100% + 20%);
  z-index: -2;
}

#app {
  background: rgba(0, 0, 0, 0.5);
  position: relative;
  top: 0;
  right: 0;
  z-index: 5;
}
</style>
