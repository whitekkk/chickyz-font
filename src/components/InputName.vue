<template>
  <div style="display: inline;">
    <input v-model="myAvatar.name" type="text" placeholder="Guest">
    <button class="button play" @click="letPlay">play</button>
    <br>
    <br>
  </div>
</template>

<script>
export default {
  props: ['myAvatar', 'letPlay']
}
</script>
