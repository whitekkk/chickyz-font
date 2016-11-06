<template>
  <div v-show="!wait">
    <part-of-game :mousePosition="mousePosition" :avatars="avatars" :myAvatar="myAvatar" :halfHeight="halfHeight" :halfWidth="halfWidth" :target="target" :foods="foods" :ranking="ranking"></part-of-game>
  </div>
</template>

<script>
import PartOfGame from './PartOfGame'

export default {
  props: ['wait', 'mousePosition', 'avatars', 'myAvatar', 'halfHeight', 'halfWidth', 'target', 'foods', 'ranking'],
  components: {
    PartOfGame
  }
}
</script>
