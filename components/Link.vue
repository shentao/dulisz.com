<template lang="html">
  <span>
    <nuxt-link
      v-if="isLocal"
      :to="to"
      class="link"
    >
      <span class="content">
        <slot/>
      </span>
      <span class="underline"/>
    </nuxt-link>
    <a
      v-else
      :href="to"
      target="_blank"
      class="link"
    >
      <span class="content">
        <slot/>
      </span>
      <span class="underline"/>
    </a>
  </span>
</template>

<script>
export default {
  props: {
    to: {
      type: String,
      default: '/'
    }
  },
  computed: {
    isLocal () {
      return !this.to.includes('https://')
    }
  }
}
</script>

<style lang="css">
.link {
  position: relative;
  text-decoration: none;
  display: inline-block;
  font-weight: 700;
  cursor: pointer;
  color: var(--text-color, #444a51);
}

.content {
  z-index: 10;
  text-shadow: 0 3px var(--background-color, #ffffff), -2px 1px var(--background-color, #ffffff), 2px 1px var(--background-color, #ffffff);
}

.underline {
  transition: all ease .2s;
  position: absolute;
  bottom: -1px;
  display: block;
  width: 100%;
  border-radius: 10px;
  height: 5px;
  background: var(--accent-color, #38c172);
  opacity: 0.5;
  z-index: -1;
}

.link:hover .underline {
  opacity: 1;
}
</style>
