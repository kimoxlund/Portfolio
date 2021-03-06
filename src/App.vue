<template>
  <div id="app" class="ui-view" :style="gradient">
    <nav-toggle
      :navToggled="navOpen"
      @onClick="navOpen = !navOpen"
    ></nav-toggle>
    <div class="ui-nav" :style="navPush">
      <transition appear :css="false" @enter="navEnter">
        <navigation></navigation>
      </transition>
    </div>

    <transition appear :css="false" mode="out-in" @enter="enter" @leave="leave">
      <router-view />
    </transition>

    <!-- <background-image></background-image> -->
  </div>
</template>

<script>
import { gsap } from "gsap";
// import BackgroundImage from "./components/BackgroundImage";

export default {
  name: "App",
  components: {
    // "background-image": BackgroundImage,
  },

  data() {
    return {
      navOpen: false,
      navTransform: -100,
      hue: this.$route.meta.h,
      saturation: this.$route.meta.s,
      luminance: this.$route.meta.l,
      opacity: 1,
    };
  },

  created() {
    this.hue = 188;
    this.luminance = 40;
    this.saturation = 35;
  },

  methods: {
    enter(el, done) {
      gsap.set(el, { opacity: 0, y: 30 });
      gsap.to(el, {
        opacity: 1,
        y: 0,
        duration: 0.5,
        ease: "sine.out",
        onComplete: done,
      });
    },

    leave(el, done) {
      gsap.to(el, {
        opacity: 0,
        y: -30,
        duration: 0.5,
        ease: "sine.in",
        onComplete: done,
      });
    },

    navEnter(el, done) {
      gsap.set(el, { opacity: 0 });
      gsap.to(el, {
        opacity: 1,
        y: 0,
        duration: 1,
        delay: 0.5,
        ease: "sine.out",
        onComplete: done,
      });
    },
  },

  computed: {
    state() {
      return this.$store.state;
    },

    isProject() {
      return this.$route.meta;
    },

    isScrolled() {
      return this.state.scrolled;
    },

    gradient() {
      return {
        background: `radial-gradient(farthest-corner at -15% -25%, hsl(${this.hue}, ${this.saturation}%, ${this.luminance}%) 0%, var(--blue-5) 80%)`,
      };
    },

    navPush() {
      return {
        transform: `translateY(${this.navTransform}%`,
      };
    },
  },

  watch: {
    $route() {
      this.navOpen = false;
    },

    isProject: function(value) {
      gsap.to(this.$data, {
        hue: value.h.toFixed(0),
        saturation: value.s,
        luminance: value.l,
        duration: 3,
      });
    },

    isScrolled: function(value) {
      value
        ? gsap.to(this.$data, {
            luminance: this.$route.meta.l - 10,
            duration: 2,
          })
        : gsap.to(this.$data, {
            luminance: this.$route.meta.l,
            duration: 2,
          });
    },

    navOpen: function(value) {
      value
        ? gsap.to(this.$data, {
            navTransform: 0,
            duration: 0.5,
          })
        : gsap.to(this.$data, {
            navTransform: -100,
            duration: 0.5,
            delay: 0.5,
          });
    },
  },
};
</script>

<style>
:root {
  --blue-5: hsl(227, 55%, 18%);
  --blue-4: hsl(229, 50%, 46%);
  --blue-3: hsl(229, 100%, 67%);
  --blue-2: hsl(219, 53%, 64%);
  --blue-1: hsl(217, 53%, 93%);
  --purple: hsl(264, 27%, 32%);
  --teal: hsl(188, 41%, 35%);
  --white: hsl(217, 53%, 83%);

  --font-size-base: 13px;
  --font-size-h1: 2.5rem;
  --font-size-h2: 1.75rem;
  --line-height-base: 1.75;
  --line-height-heading: 1.45;

  --nav-width: 100%;
  --nav-right: 0;

  --page-top: 10vh;
  --page-left: 5%;
  --page-right: 5%;
  --page-content-top: 10vh;
  --page-image-spacing: 2rem;
  --page-image-indentation: 0;
  --page-image-caption-padding-x: 3vw;
  --page-image-caption-padding-y: 2vw;
}

@media only screen and (min-width: 800px) {
  :root {
    --font-size-base: 14px;

    --nav-width: 50%;
    --nav-right: 0;

    --page-top: 8%;
    --page-left: 5%;
    --page-right: 0;
    --page-width: 60vw;
    --page-width-narrow: 40vw;
    --page-header-width: 36rem;
    --page-image-spacing: 3rem;
    --page-image-indentation: 0;

    --page-content-top: 20vh;
  }
}

@media only screen and (min-width: 980px) {
  :root {
    --nav-width: 45vw;

    --page-top: 8%;
    --page-left: 5%;
    --page-right: 0;

    --font-size-h1: 3rem;
  }
}

@media only screen and (min-width: 1200px) {
  :root {
    --bg-width: 60%;

    --page-width: 50vw;
    --page-header-width: 46rem;
    --page-indentation: 4rem;
    --page-image-spacing: 5rem;
    --page-image-indentation: 0rem;
    --page-image-caption-margin-x: 6vw;
    --page-image-caption-margin-y: 2vw;
  }
}

@media only screen and (min-width: 2000px) {
  :root {
    --font-size-base: 17px;
    --page-top: 10rem;
    --page-left: 15vw;
    --page-right: 10%;
    --page-width: 40vw;
    --page-width-narrow: 30vw;
    --page-header-width: 46rem;
    --page-indentation: 5rem;
    --page-content-top: 25vh;
    --page-image-spacing: 5rem;
    --page-image-indentation: 0rem;
    --page-image-caption-margin-x: 5rem;
    --page-image-caption-margin-y: 2rem;
    --page-image-caption-padding-x: 3rem;
    --page-image-caption-padding-y: 2rem;
    --nav-right: 5vw;
  }
}

*,
*:after,
*:before {
  box-sizing: border-box;
}

/* *[class^="ui"] {
  outline: 1px solid darkcyan;
} */

/* *[class^="page"] {
  outline: 1px solid goldenrod;
} */

/* *[class^="nav"] {
  outline: 1px solid tomato;
} */

html,
body {
  min-height: 100vh;
}

html {
  font-size: var(--font-size-base);
  overflow: hidden;
}

body {
  margin: 0;
  font-family: "work-sans", sans-serif;
  font-weight: 400;
  line-height: var(--line-height-base);
  color: var(--blue-1);
  background-color: var(--blue-5);
}

h1 {
  margin-bottom: 3rem;
  font-family: titling-gothic-fb, sans-serif;
  font-size: var(--font-size-h1);
  font-weight: 300;
  line-height: var(--line-height-heading);
}

h2 {
  font-family: titling-gothic-fb, sans-serif;
  margin: 3rem 0 1rem 0;
  font-size: 1.75rem;
  font-weight: 300;
  line-height: var(--line-height-heading);
}

p {
  margin-bottom: 1.75rem;
  font-size: 1.125rem;
  font-weight: 300;
}

img {
  width: 100%;
  height: auto;
}

.lead {
  font-size: 1.4rem;
  font-weight: 300;
  line-height: 1.65;
}

.list-inline {
  display: flex;
  align-items: center;
  list-style: none;
  margin: 0;
  padding: 0;
}

.ui-view {
  width: 100%;
  height: 100%;
  position: absolute;
}

.ui-nav {
  transform: translateY(0%);
  position: absolute;
  z-index: 30;
  width: 100%;
  height: 100%;
  background: var(--blue-5);
}

.u-text-blue-1 {
  color: var(--blue-1) !important;
}

.u-text-blue-2 {
  color: var(--blue-2) !important;
}

.u-sr-only {
  position: absolute !important;
  width: 1px !important;
  height: 1px !important;
  padding: 0 !important;
  margin: -1px !important;
  overflow: hidden !important;
  clip: rect(0, 0, 0, 0) !important;
  white-space: nowrap !important;
  border: 0 !important;
}

.ml-3 {
  margin-left: 3rem !important;
}

.pl-1 {
  padding-left: 1rem !important;
}

@media only screen and (min-width: 800px) {
  .ui-nav {
    position: relative;
    z-index: 30;
    width: auto;
    height: auto;
    background: none;
  }
}
</style>
