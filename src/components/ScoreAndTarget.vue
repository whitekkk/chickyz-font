<template>
  <div style="text-align: center;">
    <h5 class="score" style="left:140px; bottom: 0%;">
          Target :
        </h5>
        <div class="target" :style="{'background': target}">
        </div>

    <h5 class="score">
          Score : {{myAvatar.score}}
        </h5>
  </div>
</template>

<script>
export default {
  props: ['target', 'myAvatar']
}
</script>
