<template>
  <div class="workshop-card content-box" :class="{'workshop-card--has-workshoper' : workshop.workshoper}">
    <div class="workshop-card__header">
      <g-image alt="Cover image" v-if="workshop.coverImage" class="workshop-card__image" :src="workshop.coverImage" />
    </div>
    <div class="workshop-card__content">
      <div class="workshop-card__meta">
        <a
          class="workshop-card__event"
          :href="workshop.eventUrl"
          target="_blank"
        >
          {{ workshop.event }}
        </a>
        <div class="workshop-card__date">{{ workshop.date }}</div>
      </div>
      <a :href="workshop.eventUrl" style="text-decoration: none;" target="_blank">
        <h3 class="workshop-card__title">
          {{ workshop.title }}
        </h3>
      </a>
      <div class="workshop-card__location">{{ workshop.location }}</div>
      <!-- <p class="workshop-card__description" v-html="workshop.description" /> -->
    </div>
  </div>
</template>

<script>
export default {
  props: ['workshop'],
}
</script>

<style lang="scss">
.workshop-card {
  margin-bottom: 30px;
  position: relative;
  background: #fff;
  color: #333;
  box-shadow: var(--box-shadow);

  &:hover {
    box-shadow: var(--box-shadow-hover);
  }

  @media only screen and (min-width: 600px)  {
    display: flex;
  }

  &__header {
    border-radius: var(--radius) var(--radius) 0 0;
    text-align: center;
    margin-bottom: .5rem;

    @media only screen and (min-width: 600px)  {
      margin-bottom: 0;
    }

    &:empty {
      display: none;
    }
  }

  &__content {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: .5rem;

    @media only screen and (min-width: 600px)  {
      padding: 0;
    }
  }

  &__title {
    margin-top: 0;
    text-decoration: none;
    margin: 0.5rem 0;
    color: #333;

    &:hover {
      color: var(--link-color);
    }
  }

  &__meta {
    text-transform: uppercase;

    @media only screen and (min-width: 600px)  {
      align-items: center;
      display: flex;
      justify-content: space-between;
    }
  }

  &__event {
    font-weight: 700;
  }

  &__event,
  &__location {
    letter-spacing: .6px;
  }


  @media only screen and (min-width: 600px)  {
    &__event,
    &__location {
      font-size: .8em;
    }
  }

  &__location {
    opacity: .7;
  }

  &__date {
    font-size: .7em;
  }

  &__link {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0.0;
    overflow: hidden;
    text-indent: -9999px;
    z-index: 0;
  }
}
</style>
