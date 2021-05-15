<template>
  <div class="container column">
    <app-form @addBlock="addBlock"></app-form>

    <div class="card card-w70">
      <app-block v-for="block in blocks" :type="block.type" :text="block.text"></app-block>
      <h3 v-if="!blocks.length">Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>

  <div class="container">
    <p>
      <button class="btn primary" @click.prevent="loadComments">Загрузить комментарии</button>
    </p>
    <div class="card" v-if="comments.length">
      <h2>Комментарии</h2>
      <ul class="list">
        <app-comment v-for="comment in comments" :key="comment.id" :email="comment.email"
                     :body="comment.body"></app-comment>
      </ul>
    </div>
    <div class="loader" v-if="loading"></div>
  </div>
</template>

<script>
import AppBlock from "@/components/AppBlock";
import AppForm from "@/components/AppForm";
import AppComment from "@/components/AppComment";

export default {
  data() {
    return {
      url: 'https://jsonplaceholder.typicode.com/comments?_limit=42',
      blocks: [],
      comments: [],
      loading: false
    }
  },
  methods: {
    addBlock(type, text) {
      this.blocks.push({
        type,
        text
      })
    },
    async loadComments() {
      this.loading = true
      try {
        const res = await fetch(this.url, {
          method: 'GET',
          headers: {
            'Content-Type': 'application/json'
          }
        })
        this.comments = await res.json()
        this.loading = false
      } catch (e) {
        console.log(e.message)
        this.loading = false
      }
    }
  },
  components: {
    AppComment,
    AppForm,
    AppBlock
  }
}
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
