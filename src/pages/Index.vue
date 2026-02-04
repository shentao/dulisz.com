<template>
  <Layout
    :show-logo="false"
    :show-footer="false"
    :show-newsletter="false"
  >
    <div class="container content-box">
      <h1 class="title">
        <Greeting
          :day-time="dayTime"
          :language="language"
        >
          Good morning
        </Greeting> Damian here.
      </h1>
      <h2 class="subtitle">
        I’m a JavaScript developer and a <SpecialLink to="https://vuejs.org/">Vue.js</SpecialLink> core team emeriti.<br>
      </h2>
      <p class="paragraph">
        Currently the Platform Engineering Manager at <SpecialLink to="https://www.coursedog.com/">Coursedog</SpecialLink>.
      </p>
      <p class="paragraph">
        I <SpecialLink to="https://github.com/shentao">build</SpecialLink> open-source, occasionaly <SpecialLink to="/blog">write</SpecialLink> on tech, speak at events and teach Vue.js <SpecialLink to="/workshops">workshops</SpecialLink>.<br/>
      </p>
      <p class="paragraph">
        You can find me on <SpecialLink to="https://www.linkedin.com/in/damiandulisz/">LinkedIn</SpecialLink>.
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
      <p class="paragraph paragraph-small">Like my work?<br/><SpecialLink to="https://github.com/users/shentao/sponsorship">Support me on GitHub</SpecialLink>.</p>
    </div>
  </Layout>
</template>

<script>
import SpecialLink from '~/components/Link'
import Greeting from '~/components/Greeting'

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
  metaInfo: {
    title: 'About',
    meta: [
      {
        name: 'description',
        content: 'Hi, I’m Damian and I’m a JavaScript developer and a Vue.js core team member.'
      },
      {
        key: 'og:title',
        name: 'og:title',
        content: 'Damian Dulisz',
      },
      {
        key: 'twitter:title',
        name: 'twitter:title',
        content: 'Damian Dulisz',
      },
      {
        key: 'og:url',
        name: 'og:url',
        content: 'https://dulisz.com/',
      },
      {
        key: 'twitter:card',
        name: 'twitter:card',
        content: 'summary',
      },
      {
        key: 'twitter:creator',
        name: 'twitter:creator',
        content: '@damiandulisz',
      },
      {
        key: 'og:description',
        name: 'og:description',
        content: 'Hi, I’m Damian and I’m a JavaScript developer and a Vue.js core team member.',
      },
      {
        key: 'twitter:description',
        name: 'twitter:description',
        content: 'Hi, I’m Damian and I’m a JavaScript developer and a Vue.js core team member.',
      },
    ]
  },
  data () {
    return {
      dayTime: MORNING,
      language: 'en'
    }
  },
  mounted () {
    setTimeout(() => {
      this.dayTime = getDayTime()
      this.language = navigator.language.substr(0, 2)
      //add support for swiss german
      let locale = navigator.language
      if(locale === 'de-CH') this.language = 'de_CH'
    }, 2000)
  },
}
</script>

<style lang="scss">
.container {
  min-height: calc(100vh - 100px);
  display: flex;
  margin: 0 auto;
  flex-direction: column;
  justify-content: center;

  a:hover {
    opacity: 1;
  }
}

.title {
  font-size: 1.8rem;
  font-weight: 700;
  margin-bottom: 30px;
}

.subtitle {
  font-size: 1.4rem;
  font-weight: 300;
  margin-bottom: 20px;
  margin-top: 0;
  line-height: 2.25rem;
  max-width: 550px;
}

.paragraph {
 	font-family: "Lato", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  font-size: 1.1rem;
  margin-bottom: 20px;
  max-width: 750px;
}

@media only screen and (min-width: 600px)  {
  .container {
    min-height: calc(100vh - 200px);
  }

  .title {
    font-size: 2.4rem;
  }

  .subtitle {
    font-size: 1.8rem;
    margin-bottom: 30px;
    margin-top: 0;
    line-height: 2.4rem;
  }

  .paragraph {
    font-size: 1.4rem;
    margin-bottom: 30px;
  }

  .paragraph-small {
    font-size: 0.8rem;
  }
}

a.mail-link {
  display: inline-block;
  text-decoration: none;
  color: var(--body-color, #444a51);
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
