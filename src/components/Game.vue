<template>
  <div v-show="!wait">
    <part-of-game :mousePosition="mousePosition" :move="move" :time="time" :outOfArea="outOfArea" :avatars="avatars" :myAvatar="myAvatar" :halfHeight="halfHeight" :halfWidth="halfWidth" :target="target" :foods="foods"></part-of-game>
  </div>
</template>

<script>
import PartOfGame from './PartOfGame'

export default {
  props: ['wait', 'mousePosition', 'move', 'time', 'outOfArea', 'avatars', 'myAvatar', 'halfHeight', 'halfWidth', 'target', 'foods'],
  components: {
    PartOfGame
  }
}
</script>
