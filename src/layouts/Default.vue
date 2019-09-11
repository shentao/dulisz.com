<template>
  <div id="app">

    <header class="header">
      <div class="header__left">
        <Logo v-if="showLogo" />
      </div>

      <div class="header__right">
        <g-link to="/">About</g-link>
        <g-link to="/blog">Blog</g-link>
        <g-link to="/workshops">Workshops</g-link>
        <ToggleTheme />
      </div>
    </header>

    <main class="main">
      <slot/>
    </main>

    <SignupForm v-if="showNewsletter"/>
    <footer v-if="showFooter" class="footer">
      <span class="footer__copyright">Copyright © {{ new Date().getFullYear() }}. Damian Dulisz</span>
      <span class="footer__links">Powered by <a href="//gridsome.org"> Gridsome </a></span>
      <span class="footer__links"><g-link to="/privacy"> Privacy Policy</g-link></span>
    </footer>

  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import ToggleTheme from '~/components/ToggleTheme.vue'
import SignupForm from '~/components/SignupForm'

export default {
  components: {
    Logo,
    ToggleTheme,
    SignupForm
  },
  props: {
    showLogo: {
      type: Boolean,
      default: true
    },
    showFooter: {
      type: Boolean,
      default: true
    },
    showNewsletter: {
      type: Boolean,
      default: true
    }
  },
}
</script>

<style lang="scss">
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  min-height: var(--header-height);
  padding: 0 calc(var(--space) / 2);
  top:0;
  z-index: 10;

  a {
    font-size: 0.8rem;
    margin-top: -5px;
    margin-right: 20px;
    color: var(--body-color);

    &:hover {
      color: var(--link-color);
    }
  }

  &__left,
  &__right {
    display: flex;
    align-items: center;
  }

  @media screen and (min-width: 1300px) {
    //Make header sticky for large screens
    position: sticky;
    width: 100%;
  }
}

.main {
  margin: 0 auto;
  padding: 1.5vw 15px 0;
}

.footer {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: calc(var(--space) / 2);
  text-align: center;
  font-size: .8em;

  > span {
    margin: 0 .35em;
  }

  a {
    color: currentColor;
  }
}
</style>
