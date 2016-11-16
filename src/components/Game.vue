<template>
  <div v-show="!wait">
    <part-of-game :mousePosition="mousePosition" :avatars="avatars" :myAvatar="myAvatar" :halfHeight="halfHeight" :halfWidth="halfWidth" :target="target" :foods="foods" :hOFs="hOFs" :ranking="ranking" :mapSize="mapSize" :mapResize="mapResize" :countStep="countStep" :maxStep="maxStep"></part-of-game>
  </div>
</template>

<script>
import PartOfGame from './PartOfGame'

export default {
  props: ['wait', 'mousePosition', 'avatars', 'myAvatar', 'halfHeight', 'halfWidth', 'target', 'foods', 'hOFs', 'ranking', 'mapSize', 'mapResize', 'countStep', 'maxStep'],
  components: {
    PartOfGame
  }
}
</script>
