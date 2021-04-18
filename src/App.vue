<template>
  <div id="window">
    <main>
      <NavBar />
      <div id="window__content">
        <TitleBar />
        <router-view />
      </div>
    </main>
    <Footer />
  </div>
</template>

<script lang="ts">
import Footer from "@/components/Footer.vue";
import NavBar from "@/components/NavBar.vue";
import TitleBar from "@/components/TitleBar.vue";
import { Options as Component, Vue } from "vue-class-component";
import { registerServices } from "@/services/service-container";

@Component({
  components: {
    Footer,
    NavBar,
    TitleBar
  }
})
export default class App extends Vue {
  async created() {
    registerServices();
  }
}
</script>

<style lang="scss">
@import "~@/assets/scss";

$size-scrollbar: 16px;
$colour-scrollbar-arrows: white;

body {
  margin: 0;
}

main {
  display: flex;
  flex-direction: row;
  flex-grow: 1;
}

p {
  font-weight: 300;
  line-height: 30px;
  margin: 0;
  size: 14px;
}

#app {
  color: $colour-text;
}

#window {
  // The url below is a placeholder until we get a system for user generated images
  background-image: linear-gradient(90deg, #000000aa 20%, #00000000 70%),
    url(https://picsum.photos/1200/600);
  display: flex;
  flex-direction: column;
  height: $size-window-height;
  width: $size-window-width;
}

#window__content {
  display: flex;
  flex-direction: column;
  width: 100%;
}

// Custom scrollbars

::-webkit-scrollbar {
  width: $size-scrollbar;
  background-color: $colour-background--darker;
}

::-webkit-scrollbar-thumb {
  background-color: $colour-background--dark;
}

/* Buttons */
::-webkit-scrollbar-button:single-button {
  height: $size-scrollbar;
  width: $size-scrollbar;

  display: block;
  background-size: 16px;
  background-repeat: no-repeat;
  background-position: center;
}

// Up arrow
::-webkit-scrollbar-button:single-button:vertical:decrement {
  background-image: url('data:image/svg+xml;utf-8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="#{$colour-scrollbar-arrows}"><path d="M0 0h24v24H0V0z" fill="none"/><path d="M12 8l-6 6 1.41 1.41L12 10.83l4.59 4.58L18 14l-6-6z"/></svg>');
}

::-webkit-scrollbar-button:vertical:single-button:decrement:hover,
::-webkit-scrollbar-button:vertical:single-button:increment:hover {
  background-color: $colour-background--dark;
}

// Down arrow
::-webkit-scrollbar-button:single-button:vertical:increment {
  background-image: url('data:image/svg+xml;utf-8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="#{$colour-scrollbar-arrows}"><path d="M24 24H0V0h24v24z" fill="none"/><path d="M16.59 8.59L12 13.17 7.41 8.59 6 10l6 6 6-6-1.41-1.41z"/></svg>');
}
</style>
