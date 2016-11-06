<template>
  <div v-show="!wait">
    <part-of-game :mousePosition="mousePosition" :time="time" :avatars="avatars" :myAvatar="myAvatar" :halfHeight="halfHeight" :halfWidth="halfWidth" :target="target" :foods="foods" :ranking="ranking"></part-of-game>
  </div>
</template>

<script>
import PartOfGame from './PartOfGame'

export default {
  props: ['wait', 'mousePosition', 'time', 'avatars', 'myAvatar', 'halfHeight', 'halfWidth', 'target', 'foods', 'ranking'],
  components: {
    PartOfGame
  }
}
</script>
