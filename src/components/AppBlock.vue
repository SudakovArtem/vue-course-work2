<template>
  <div class="" v-html="renderBlock(type, text)"></div>

  <!--  можно было сделать так:
    <h1 v-if="type === 'title'">{{ text }}</h1>
    <div v-else-if="type === 'avatar'" class="avatar">
      <img :src="text" alt="avatar">
    </div>
    <h2 v-else-if="type === 'subtitle'">{{ text }}</h2>
    <p v-else-if="type === 'text'">
      {{text}}
    </p>-->
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      validator: function (value) {
        return ['title', 'avatar', 'subtitle', 'text'].indexOf(value) !== -1
      }
    },
    text: {
      type: String
    }
  },
  name: "AppBlock",
  methods: {
    renderBlock(type, text) {
      if (type === 'title') {
        return `<h1>${text}</h1>`
      } else if (type === 'avatar') {
        return `<div class="avatar">
          <img src="${text}" alt="avatar">
        </div>`
      } else if (type === 'subtitle') {
        return `<h2>${text}</h2>`
      } else if (type === 'text') {
        return `<p>${text}</p>`
      }
    }
  }
}
</script>

<style scoped>

</style>
