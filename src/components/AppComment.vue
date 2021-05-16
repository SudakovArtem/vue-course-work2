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
  props: {
    email: {
      type: String,
      required: true
    },
    body: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>

</style>
