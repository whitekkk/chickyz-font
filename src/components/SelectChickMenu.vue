<template>
  &nbsp;
  <div style="margin: 0 auto; display: inline-block;">
    <button @click="selectFace(-1)" class="button play">&lt;</button> Face : {{ f }}
    <button @click="selectFace(1)" class="button play">&gt;</button>
    <br>
    <button @click="selectColor(-1)" class="button play">&lt;</button> Color : {{ c }}
    <button @click="selectColor(1)" class="button play">&gt;</button>
  </div>
</template>

<script>
export default {
  props: ['selectFace', 'f', 'selectColor', 'c']
}
</script>
