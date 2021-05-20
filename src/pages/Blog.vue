<template>
  <Layout>
    <section v-if="$page">
      <ul>
        <li v-for="{ node } in $page.posts.edges" :key="node.id">
          <h2>
            <g-link :to="node.path">{{ node.title }}</g-link>
          </h2>
          <div>
            <span>{{ node.date }}</span>
          </div>
          <div>
            {{ node.excerpt }}
          </div>
          <div>
            <g-link :to="node.path">Read More</g-link>
          </div>
        </li>
      </ul>
      <pager
        v-if="$page.posts.pageInfo.totalPages > 1"
        :info="$page.posts.pageInfo"
      />
    </section>
  </Layout>
</template>

<page-query>
  query Posts($page: Int) {
    posts: allContentfulBlogPost( sortBy: "date", order:DESC, perPage: 3, page: $page ) @paginate {
      totalCount pageInfo {
        totalPages
        currentPage
      } edges {
        node {
          id
          title
          path
          date(format:"MMMM D, Y")
        }
      }
    }
  }
</page-query>

<script>
  import { Pager } from 'gridsome'

  export default {
    metaInfo: {
      title: 'Blog',
    },
    components: {
      Pager,
    },
  }
</script>
