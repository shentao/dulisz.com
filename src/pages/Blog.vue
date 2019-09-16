<template>
  <Layout :show-logo="false">
    <!-- Author intro -->
    <Author :show-title="true" />

    <!-- List posts -->
    <div class="posts">
      <PostCard v-for="edge in $page.posts.edges" :key="edge.node.id" :post="edge.node"/>
    </div>

  </Layout>
</template>

<page-query>
{
  posts: allPost {
    edges {
      node {
        id
        title
        path
        tags {
          id
          title
          path
        }
        date (format: "D. MMMM YYYY")
        timeToRead
        description
        ...on Post {
            id
            title
            path
        }
      }
    }
  }
}
</page-query>

<script>
import Author from '~/components/Author.vue'
import PostCard from '~/components/PostCard.vue'

export default {
  components: {
    Author,
    PostCard
  },
  metaInfo: {
    title: 'Blog',
    meta: [
      {
        name: 'description',
        content: 'A blog about Vue.js by Damian Dulisz',
      },
      {
        key: 'og:title',
        name: 'og:title',
        content: 'Blog',
      },
      {
        key: 'twitter:title',
        name: 'twitter:title',
        content: 'Blog',
      },
      {
        key: 'og:description',
        name: 'og:description',
        content: 'A blog about Vue.js by Damian Dulisz',
      },
      {
        key: 'twitter:description',
        name: 'twitter:description',
        content: 'A blog about Vue.js by Damian Dulisz',
      },
    ]
  }
}
</script>
