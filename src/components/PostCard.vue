<template>
  <div class="post-card content-box" :class="{'post-card--has-poster' : post.poster}">
    <div class="post-card__header">
      <!-- <g-image alt="Cover image" v-if="post.coverImage" class="post-card__image" :src="post.coverImage" /> -->
    </div>
    <div class="post-card__content">
      <g-link :aria-label="`view '${post.title}'`" :to="post.path" style="text-decoration: none;">
        <h2 class="post-card__title" v-html="post.title" />
      </g-link>
      <PostMeta class="post-card__meta" :post="post" />
      <p class="post-card__description" v-html="post.description" />
      <g-link :aria-label="`view '${post.title}'`" :to="post.path" class="post-card__read-more">Read →</g-link>

      <!-- <PostTags class="post-card__tags" :post="post" /> -->
    </div>
  </div>
</template>

<script>
import PostMeta from '~/components/PostMeta'
import PostTags from '~/components/PostTags'

export default {
  components: {
    PostMeta,
    PostTags
  },
  props: ['post'],
}
</script>

<style lang="scss">
.post-card {
  margin-bottom: 30px;
  position: relative;

  &__header {
    margin-left: calc(var(--space) * -1);
    margin-right: calc(var(--space) * -1);
    margin-bottom: calc(var(--space) / 2);
    margin-top: calc(var(--space) * -1);
    overflow: hidden;
    border-radius: var(--radius) var(--radius) 0 0;

    &:empty {
      display: none;
    }
  }

  &__image {
    min-width: 100%;
  }

  &__title {
    margin-top: 0;
    margin-bottom: 0.5rem;
    text-decoration: none;

    &:hover {
      color: var(--link-color);
    }
  }

  &__tags {
    z-index: 1;
    position: relative;
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

  &__description {
    margin-bottom: 0.5rem;
  }

  &__read-more {
    display: inline-block;
    font-size: 0.9em;
    margin-bottom: 1rem;
    font-weight: 700;
  }
}
</style>
