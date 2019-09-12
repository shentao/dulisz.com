<template>
  <Layout>
    <div class="content-box">
      <h1 class="post-title__text">
        {{ $page.post.title }}
      </h1>

      <PostMeta :post="$page.post" />

    </div>

    <div class="post content-box">
      <div class="post__header">
        <g-image alt="Cover image" v-if="$page.post.coverImage" :src="$page.post.coverImage" />
      </div>

      <div class="post__content" v-html="$page.post.content" />

      <div class="post__footer">
        <PostTags :post="$page.post" />
      </div>
      <div class="post-comments">
        <a :href="discussUrl" target="_blank">
          Discuss on Twitter
        </a>
        <span style="padding: 0 8px;">•</span>
        <a :href="editUrl" target="_blank">
          Edit on GitHub
        </a>
      </div>
    </div>

    <Author class="post-author" :show-title="true" />
  </Layout>
</template>

<script>
import PostMeta from '~/components/PostMeta'
import PostTags from '~/components/PostTags'
import Author from '~/components/Author.vue'

export default {
  components: {
    Author,
    PostMeta,
    PostTags
  },
  metaInfo () {
    return {
      title: this.$page.post.title,
      meta: [
        {
          name: 'description',
          content: this.$page.post.description
        }
      ]
    }
  },
  computed: {
    discussUrl () {
      return `https://mobile.twitter.com/search?q=${encodeURIComponent(`https://dulisz.com${this.$page.post.path}`)}`
    },
    editUrl () {
      const file = this.$page.post.path.slice(6, this.$page.post.path.length)
      console.log(file);
      return `https://github.com/shentao/dulisz.com/edit/master/content/posts/${file}.md`;
    }
  }
}
</script>

<page-query>
query Post ($path: String!) {
  post: post (path: $path) {
    title
    path
    date (format: "D. MMMM YYYY")
    timeToRead
    tags {
      id
      title
      path
    }
    description
    content
  }
}
</page-query>

<style lang="scss">
.post {

  &__header {
    width: calc(100% + var(--space) * 2);
    margin-left: calc(var(--space) * -1);
    margin-bottom: calc(var(--space) / 2);
    overflow: hidden;
    border-radius: var(--radius) var(--radius) 0 0;

    img {
      width: 100%;
    }

    &:empty {
      display: none;
    }
  }

  &__content {
    h2:first-child {
      margin-top: 0;
    }

    p:first-of-type {
      font-size: 1.2em;
      color: var(--title-color);
    }

    img {
      width: calc(100% + var(--space) * 2);
      margin-left: calc(var(--space) * -1);
      display: block;
      max-width: none;
    }
  }
}

.post-comments {
  margin-top: 2rem;

  a {
    font-weight: 700;
    border-bottom: 1px solid var(--link-color)
  }

  &:empty {
    display: none;
  }
}

.post-author {
  margin-top: calc(var(--space) / 2);
}
</style>
