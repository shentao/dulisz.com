<script>
import SpecialLink from '@/components/Link'
import Greeting from '@/components/Greeting'

const EARLY_MORNING = 'EARLY_MORNING'
const MORNING = 'MORNING'
const AFTERNOON = 'AFTERNOON'
const EVENING = 'EVENING'

function getDayTime () {
  const hourNow = new Date(Date.now()).getHours()

  switch (true) {
    case (hourNow >= 5 && hourNow < 7):
      return EARLY_MORNING
    case (hourNow >= 12 && hourNow < 17):
      return AFTERNOON
    case (hourNow >= 17 || hourNow < 5):
      return EVENING
    default:
      return MORNING
  }
}

export default {
  components: { SpecialLink, Greeting },
  data () {
    return {
      dayTime: MORNING,
      language: 'en'
    }
  },
  computed: {
    theme () {
      return EVENING === this.dayTime
        ? { '--background-color': '#22292f', '--text-color': '#fff', '--accent-color': '#51d88a' }
        : { '--background-color': '#fff', '--text-color': '#444a51', '--accent-color': '#38c172' }
    }
  },
  watch: {
    theme (theme) {
      for (const property in theme) {
        document.body.style.setProperty(property, theme[property])
      }
    }
  },
  mounted () {
    setTimeout(() => {
      this.dayTime = getDayTime()
      this.language = navigator.language.substr(0, 2)
    }, 2000)
  },
}
</script>

<template>
  <div class="container">
    <h1 class="title">
      <Greeting
        :day-time="dayTime"
        :language="language"
      >
        Good morning
      </Greeting> Damian here.
    </h1>
    <h2 class="subtitle">
      I’m a JavaScript developer and a <SpecialLink to="https://vuejs.org/">Vue.js</SpecialLink> core team member.<br>
    </h2>
    <p class="paragraph">
      Working as a consultant for companies around the world.
    </p>
    <p class="paragraph">
      I <SpecialLink to="https://github.com/shentao">build</SpecialLink> open-source, occasionaly <SpecialLink to="https://medium.com/@DamianDulisz">write</SpecialLink> on tech and speak at events. I’m also running the official <SpecialLink to="https://news.vuejs.org">Vue.js News</SpecialLink>.
    </p>
    <p class="paragraph">
      You can follow me on <SpecialLink to="https://twitter.com/DamianDulisz">Twitter</SpecialLink>.
    </p>
    <!-- <transition
      name="resize"
      mode="out-in"
    >
      <nuxt class="router-view"/>
    </transition> -->
    <a
      href="mailto:damian@dulisz.com"
      class="paragraph mail-link"
    >
      ↳ Talk to me.
    </a>
    <p>Like my work? <SpecialLink to="https://www.buymeacoffee.com/DamianDulisz">Buy me a coffee</SpecialLink>.</p>
  </div>
</template>

<style>
html {
  font-family: "Lato", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  font-size: 16px;
  font-weight: 300;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
  font-weight: 300;
  color: var(--text-color, #444a51);
}

body {
  transition: all 0.3s ease-in-out;
  background: var(--background-color, #ffffff);
  color: var(--text-color, #444a51);
}

*, *:before, *:after {
  box-sizing: border-box;
  margin: 0;
}

.container {
  min-height: 100vh;
  max-width: 1200px;
  display: flex;
  margin: 0 auto;
  padding: 20px;
  flex-direction: column;
  justify-content: center;
}

.title {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 30px;
}

.subtitle {
  font-size: 1.75rem;
  font-weight: 300;
  margin-bottom: 20px;
  line-height: 2.25rem;
  max-width: 550px;
}

.paragraph {
  font-size: 1.5rem;
  margin-bottom: 20px;
  max-width: 750px;
}

@media only screen and (min-width: 600px)  {
  .title {
    font-size: 3rem;
  }

  .subtitle {
    font-size: 2.25rem;
    margin-bottom: 30px;
    line-height: 3rem;
  }

  .paragraph {
    font-size: 1.75rem;
    margin-bottom: 30px;
  }
}

.mail-link {
  display: inline-block;
  text-decoration: none;
  color: var(--text-color, #444a51);
}

.resize-enter-active, .resize-leave-active {
  transition: max-height .5s, opacity .4s;
  max-height: 600px;
}
.resize-enter, .resize-leave-to {
  max-height: 0;
  opacity: 0;
}
</style>
