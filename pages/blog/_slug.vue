<template>
  <section class="slug">
    <h1 class="slug_title">
      {{ article.fields.title }}
    </h1>
    <p class="slug_date">{{ article.sys.updatedAt }}</p>
    <div v-html="renderedContent" />
  </section>
</template>
<script>
import { documentToHtmlString } from '@contentful/rich-text-html-renderer'
import { createClient } from '~/plugins/contentful.js'

const client = createClient()

export default {
  props: {
    id: {
      type: String,
      default: ''
    }
  },
  transition: 'slide-right',
  async asyncData({ env, params }) {
    return await client
      .getEntries({
        'content_type': env.CTF_BLOG_POST_TYPE_ID,
        'fields.slug': params.slug
      })
      .then(entries => {
        return {
          article: entries.items[0]
        }
      })
      .catch(console.error)
  },
  computed: {
    renderedContent () {
      return documentToHtmlString(this.article.fields.body)
    }
  }
}
</script>
