<template>
  <div style="text-align: center;">
    <h5 style="position: absolute; right:140px; bottom: 4%;">
          Target
          <div class="target" :style="{'background': target}">
          </div>
        </h5>

    <h3 style="color: #FFFFFF; text-shadow: -3px 0 black, 0 3px black, 3px 0 black, 0 -3px black; font-size: 1.5em; position: absolute; right:20px; bottom: 0;">
          Score <br> {{myAvatar.score}}
        </h3>
  </div>
</template>

<script>
export default {
  props: ['target', 'myAvatar']
}
</script>
