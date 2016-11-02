<template>
  <div>
    <img v-if="myAvatar.id === avatar.id" src="../../static/img/gird-bg.png" :style="{'position':'absolute', 'top': -avatar.y + 'px', 'left': -avatar.x + 'px'}" />
    <div style="text-align: center; z-index: 100; position: absolute;">
      <a style="font-family: Lucida Console; font-weight: bolder; color: #00CC00; font-size: 1.5em;">
          {{avatar.name}}
        </a>
      <br>
      <div class="chick" :style="{'background': avatar.color}">
        <img :src="'../../static/img/' + avatar.face + '.png'" width="100%" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['avatar', 'myAvatar', 'halfHeight', 'halfWidth'],
  components: {
  }
}
</script>
