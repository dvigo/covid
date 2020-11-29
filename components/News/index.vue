<template lang="pug">
  div
    h2 Últimas notícias
    div(v-for="article in news.articles")
      v-card(class="my-3" hover data-aos="zoom-in" data-aos-easing="ease")
        v-img(height="350px" v-bind:src="article.urlToImage")
        v-container(fill-height fluid)
          v-layout
            v-flex(xs12 align-end d-flex)
              span(class="headline") {{ article.title }}
        v-chip(small color="#879F6F" class="white--text" style="margin-left: 5px") {{ new Date(article.publishedAt).toLocaleDateString('es')}}
        v-card-text {{ article.description }}
        v-card-actions
          v-chip(small color="secondary" class="white--text") {{article.source.name}}
          v-spacer
          v-btn(small replace color="info" v-bind:href="article.url" target="_blank") Leer más
</template>

<script>
export default {
  data() {
    return {
      news: []
    }
  },
  created () {
    this.fetch()
  },
  methods: {
    async fetch () {
      this.news = await this.$axios.$get('/news')
    }
  }
}
</script>