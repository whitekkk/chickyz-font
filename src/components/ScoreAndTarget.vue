<template>
  <div>
    <h5 style="position: absolute; right:150px; bottom: 4%;">
          Target
          <div class="target" :style="{'background': target}">
          </div>
        </h5>

    <h3 style="font-size: 2em; position: absolute; right:0; bottom: 0;">
          Score {{myAvatar.score}}
        </h3>
  </div>
</template>

<script>
export default {
  props: ['target', 'myAvatar']
}
</script>
