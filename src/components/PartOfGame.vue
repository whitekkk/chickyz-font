<template>
  <div @mousemove="mousePosition()">
    <div class="screen" @mouseover="move(time)" @mouseout="outOfArea">

      <div class="foods" :style="{'top': food.y-myAvatar.y+halfHeight + 'px', 'left': food.x-myAvatar.x+halfWidth + 'px'}" v-for="food in foods">
          <food :food="food"></food>
      </div>

      <div class="avatar" :style="{'top': avatar.y-myAvatar.y+(halfHeight)-75 + 'px', 'left': avatar.x-myAvatar.x+halfWidth-50 + 'px'}" v-for="avatar in avatars">
        <avatar :avatar="avatar" :myAvatar="myAvatar" :halfHeight="halfHeight" :halfWidth="halfWidth"></avatars>
      </div>

      <rank-board :avatars="avatars"></rank-board>

      <score-and-target :target="target" :myAvatar="myAvatar"></score-and-targe>

    </div>
  </div>
</template>

<script>
import Avatar from './Avatar'
import RankBoard from './RankBoard'
import Food from './Food'
import ScoreAndTarget from './ScoreAndTarget'

export default {
  props: ['mousePosition', 'move', 'time', 'outOfArea', 'avatars', 'myAvatar', 'halfHeight', 'halfWidth', 'target', 'foods'],
  components: {
    Avatar,
    RankBoard,
    Food,
    ScoreAndTarget
  }
}
</script>
