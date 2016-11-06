<template>
  <div class="miniMap">
    <div v-for="avatar in avatars" style="position: absolute;">
      <div v-if="myAvatar.id === avatar.id" class="miniAvatar" :style="{'position':'absolute','background': myAvatar.color,'top': avatar.y*0.08 + 'px', 'left': avatar.x*0.08 + 'px'}">
      </div>
      <div v-else class="miniAvatar" :style="{'position':'absolute','background': '#888888', 'top': avatar.y*0.08 + 'px', 'left': avatar.x*0.08 + 'px'}">
      </div>
    </div>
    <div v-for="food in foods">
      <div v-if="food.color !== ''" class="miniFood" :style="{'position':'absolute','background': food.color,'top': food.y*0.08 + 'px', 'left': food.x*0.08 + 'px'}">
      </div>
      <div v-else class="miniFood" :style="{'position':'absolute','background': food.color,'top': food.y*0.08 + 'px', 'left': food.x*0.08 + 'px'}">
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['avatars', 'myAvatar', 'foods']
}
</script>
