<template lang="pug">
    .main
        h1.main-title
            template(v-for="(text, i) in texts")
                span( :class="`text-${i}`")
                br(v-if="text.breakSpace")
            br.mobile-break

            span(:class="`${coloredClass} text-keyword colored `")
</template>

<script>
import Typed from "typed.js";

function getRandomArbitrary(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min)) + min;
}

export default {
  name: "App",
  data() {
    return {
      currentText: null,
      paroleChiave: ["le sfide", "le nuove tecnologie"],
      coloredClass: "",
      texts: [
        {
          text: "Ciao mi chiamo Valentin.",
          breakSpace: true
        },
        {
          text: "Mi piace&nbsp;"
        },
        {
          text: "Conoscere"
        },
        {
          text: "Imparare"
        },
        {
          text: "Sperimentar"
        },
        {
          text: "Sognare"
        },
        {
          text: "Migliorare"
        },
        {
          text: "Creare"
        },
        {
          text: "Innovare"
        },
        {
          text: "Esplorare"
        }
      ]
    };
  },

  watch: {
    currentText: {
      handler(newVal) {
        const options = {
          showCursor: true,
          typeSpeed: 30
        };
        let spanClass = `.text-${newVal}`;

        if (newVal > 1) {
          options.smartBackspace = false;
          options.backSpeed = 30;
          options.backDelay = 1000;
          options.strings = this.texts
            .slice(2, this.texts.length)
            .map(text => text.text);
          options.shuffle = false;
          options.loop = true;
          options.loopCount = 2000;
          options.onComplete = self => {};
          options.preStringTyped = self => {
            const classiCss = Array.from(
              { length: 10 },
              (v, i) => "c-" + (i + 1)
            ).filter(c => c !== this.coloredClass);
            this.coloredClass =
              classiCss[getRandomArbitrary(0, classiCss.length)];
          };
          spanClass = ".text-keyword";
        } else {
          options.strings = [this.texts[newVal].text];
          options.onComplete = self => {
            self.cursor.parentNode.removeChild(self.cursor);
            this.currentText = newVal + 1;
          };
        }
        const typed = new Typed(spanClass, options);
      }
    }
  },
  mounted() {
    this.currentText = 0;
  }
};
</script>

<style lang="sass">
    @import "assets/mixins"
    .main
        height: 100vh
        display: flex
        justify-content: center
        align-items: center
        overflow: hidden
        position: relative
        z-index: 1
        pointer-events: none

    .colored
        animation: scrollColor 10s linear infinite
        color: transparent

    .c-1
        background: linear-gradient(to right, #fc466b, #3f5efb)

    .c-2
        background: linear-gradient(to right, #e1eec3, #f05053)

    .c-3
        background: linear-gradient(to right, #00b09b, #96c93d)

    .c-4
        background: linear-gradient(to right, #3ca55c, #b5ac49)

    .c-5
        background: linear-gradient(to right, #348f50, #56b4d3)

    .c-6
        background: linear-gradient(to right, #02aab0, #00cdac)

    .c-7
        background: linear-gradient(to right, #d31027, #ea384d)

    .c-8
        background: linear-gradient(to right, #314755, #26a0da)

    .c-9
        background: linear-gradient(to right, #e65c00, #f9d423)

    .c-10
        background: linear-gradient(to right, #00467f, #a5cc82)

    .c-1, .c-2, .c-3, .c-4, .c-5, .c-6, .c-7, .c-8, .c-9, .c-10
        background-clip: text
        background-size: 200% auto

    .typed-cursor
        color: #ABABAB

    @media screen and (min-width: 600px)
        .mobile-break
            display: none


</style>
