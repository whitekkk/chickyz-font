<template>
  <div @mousemove="mousePosition()">
    <div class="screen">

      <div class="foods" :style="{'top': food.y-myAvatar.y+halfHeight + 'px', 'left': food.x-myAvatar.x+halfWidth + 'px'}" v-for="food in foods">
          <food :food="food"></food>
      </div>

      <div class="avatar" :style="{'top': avatar.y-myAvatar.y+(halfHeight)-75 + 'px', 'left': avatar.x-myAvatar.x+halfWidth-50 + 'px'}" v-for="avatar in avatars">
        <avatar :avatar="avatar" :myAvatar="myAvatar" :halfHeight="halfHeight" :halfWidth="halfWidth"></avatars>
      </div>

      <rank-board :ranking="ranking"></rank-board>

      <explan-law-in-game></explan-law-in-game>

      <score-and-target :target="target" :myAvatar="myAvatar"></score-and-targe>

    </div>
  </div>
</template>

<script>
import Avatar from './Avatar'
import RankBoard from './RankBoard'
import Food from './Food'
import ScoreAndTarget from './ScoreAndTarget'
import ExplanLawInGame from './ExplanLawInGame'

export default {
  props: ['mousePosition', 'time', 'avatars', 'myAvatar', 'halfHeight', 'halfWidth', 'target', 'foods', 'ranking'],
  components: {
    Avatar,
    RankBoard,
    Food,
    ScoreAndTarget,
    ExplanLawInGame
  }
}
</script>
