<template>
  <form class="card card-w30">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="blockType">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model="text"></textarea>
    </div>

    <button class="btn primary" :disabled="!validTextLength" @click.prevent="buttonClickHandler">Добавить</button>
  </form>
</template>

<script>
export default {
  name: "AppForm",
  emits: ['addBlock'],
  data() {
    return {
      blockType: 'title',
      text: ''
    }
  },
  methods: {
    buttonClickHandler() {
      this.$emit('addBlock', this.blockType, this.text)
      this.blockType = 'title'
      this.text = ''
    }
  },
  computed: {
    validTextLength() {
      return this.text.length >= 3
    }
  },
}
</script>

<style scoped>

</style>
