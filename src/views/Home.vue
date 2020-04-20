<template>
  <div>
    <ion-card v-for="post in posts" :key="post.data.id" style="border: 1px solid blue">
      <ion-card-content>
        <img v-if="post.data.preview" :src="decoder(post.data.preview.images[0].source.url)" alt="">
        <ion-label color="light">{{ post.data.title }}</ion-label>
        <ion-button color="tertiary" expand="full" @click="viewMore(post.data)">
          View More
        </ion-button>
      </ion-card-content>
    </ion-card>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      posts: []
    }
  },
  async mounted() {
    const res = await axios.get('https://www.reddit.com/r/ionic.json')
    this.posts = res.data.data.children
  },
  methods: {
    decoder(string) {
      const parser = new DOMParser()
      let dom = parser.parseFromString(string, 'text/html')
      return dom.body.textContent
    },
    viewMore(post) {
      this.$router.push({name: 'Detail', params: { post }})
    }
  }
}
</script>
