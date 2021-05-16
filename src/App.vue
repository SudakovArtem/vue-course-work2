<template>
  <div class="container column">
    <app-form @submit-form="addBlock"></app-form>

    <div class="card card-w70">
      <component v-for="block in blocks" :is="`app-${block.type}`" :text="block.text"></component>
      <h3 v-if="!blocks.length && !blockLoading">Добавьте первый блок, чтобы увидеть результат</h3>
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
import AppTitle from "@/components/AppTitle";
import AppAvatar from "@/components/AppAvatar";
import AppSubtitle from "@/components/AppSubtitle";
import AppText from "@/components/AppText";
import AppForm from "@/components/AppForm";
import AppComment from "@/components/AppComment";

export default {
  data() {
    return {
      getUrl: 'https://jsonplaceholder.typicode.com/comments?_limit=42',
      postUrl: 'https://vue-course-work2-default-rtdb.firebaseio.com/blocks.json',
      blocks: [],
      comments: [],
      loading: false,
      blockLoading: true,
    }
  },
  mounted() {
    this.loadBlocks()
  },
  methods: {
    async addBlock(type, text) { // добавление блока в разметку и загрузка в базу данных firebase
      const block = {
        type,
        text
      }
      try {
        await fetch(this.postUrl, {
          method: 'POST',
          body: JSON.stringify(block),
          headers: {
            'Content-Type': 'application/json'
          }
        });
        this.blocks.push(block)
      } catch (error) {
        console.error('Ошибка:', error);
      }
    },
    async loadComments() { // загрузка комментариев
      this.loading = true
      try {
        const res = await fetch(this.getUrl, {
          method: 'GET',
          headers: {
            'Content-Type': 'application/json'
          }
        })
        this.comments = await res.json()
        this.loading = false
      } catch (error) {
        console.error('Ошибка:', error);
        this.loading = false
      }
    },
    async loadBlocks() { // загрузка из базы данных firebase ранее созданных блоков
      try {
        const res = await fetch(this.postUrl)
        const data = await res.json()
        if (!data) { // если в базе данных нет ни одной записи
          this.blocks = []
          this.blockLoading = false
          return
        }
        this.blocks = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.blockLoading = false
      } catch (error) {
        console.error('Ошибка:', error);
        this.blocks = []
        this.blockLoading = false
      }
    }
  },
  components: {
    AppTitle,
    AppComment,
    AppForm,
    AppText,
    AppAvatar,
    AppSubtitle
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
