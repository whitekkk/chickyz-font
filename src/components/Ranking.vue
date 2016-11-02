<template>
  <div>
    <h6 v-if="avatar.score !== undefined">
        <div style="display: inline-block;">
          {{' '}}
        </div>
        <div style="display: inline-block;">
          &nbsp;
          {{avatar.name + ' '}}
        </div>
        <div style="position: absolute; right:0; display: inline-block;">
          {{avatar.score}}
        </div>
      </h6>
  </div>
</template>

<script>
export default {
  props: ['avatar'],
  components: {
  }
}
</script>
