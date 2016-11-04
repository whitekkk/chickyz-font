<template>
  <div style="text-align: center;">
    <h5 style="position: absolute; right:140px; bottom: 4%;">
          Target
          <div class="target" :style="{'background': target}">
          </div>
        </h5>

    <h3 style="font-size: 1.5em; position: absolute; right:20px; bottom: 0;">
          Score <br> {{myAvatar.score}}
        </h3>
  </div>
</template>

<script>
export default {
  props: ['target', 'myAvatar']
}
</script>
