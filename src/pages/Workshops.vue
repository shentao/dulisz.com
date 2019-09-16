<template>
  <Layout :show-logo="false">
    <div class="content-box">
      <h1 class="title">
        Workshops
      </h1>
      <p>
        Looking for an efficient way to boost your Vue.js skills?<br/>
        Consider joining in one of my workshops where I share my experience and more in-depth insight into working with Vue.
      </p>
      <h2 class="subtitle">
        Upcoming Workshops
      </h2>
    </div>
    <WorkshopCard
      v-for="edge in $page.workshops.edges" :key="edge.node.id"
      :workshop="edge.node"
    />
    <div class="content-box">
      <p>
        Perhaps you need a dedicated workshop for your company? <a
          href="mailto:damian@dulisz.com"
          class="mail-link"
        >Let’s talk</a>!
      </p>
    </div>

  </Layout>
</template>

<page-query>
{
  workshops: allWorkshops(sortBy: "date", order: ASC) {
    edges {
      node {
        id
        title
        location
        event
        eventUrl
        date (format: "MMMM DD, YYYY")
        description
        coverImage (width: 300, blur: 10)
      }
    }
  }
}
</page-query>

<script>
import Author from '~/components/Author.vue'
import WorkshopCard from '~/components/WorkshopCard.vue'

export default {
  components: {
    WorkshopCard
  },
  metaInfo: {
    title: 'Vue.js Workshops',
    meta: [
      {
        name: 'description',
        content: 'Vue.js Workshops by Damian Dulisz'
      },
      {
        key: 'og:title',
        name: 'og:title',
        content: 'Vue.js Workshops',
      },
      {
        key: 'twitter:title',
        name: 'twitter:title',
        content: 'Vue.js Workshops',
      },
      {
        key: 'og:description',
        name: 'og:description',
        content: 'Vue.js Workshops by Damian Dulisz',
      },
      {
        key: 'twitter:description',
        name: 'twitter:description',
        content: 'Vue.js Workshops by Damian Dulisz',
      },
    ]
  }
}
</script>

<style scoped>
.mail-link {
  color: var(--link-color);
}
</style>
