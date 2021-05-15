<template>
  <li class="list-item">
    <div>
      <p><strong>{{ email }}</strong></p>
      <small>{{ body }}</small>
    </div>
  </li>
</template>

<script>
export default {
  name: "AppComment",
  props: ['email', 'body']
}
</script>

<style scoped>

</style>
