<template>
  <div class="rankingBoard">
    <h4>&nbsp;&nbsp;
      Ranking <img src="../../static/img/crown.png"/>
      </h4>
    <div class="ranking" v-for="avatar in ranking">
      <ranking :avatar="avatar"></ranking>
    </div>
  </div>
</template>

<script>
import Ranking from './Ranking'

export default {
  props: ['ranking'],
  components: {
    Ranking
  }
}
</script>
