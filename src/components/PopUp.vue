<template>
  <div v-show="wait">
    <Background-and-logo></Background-and-logo>
    <form-befor-play :wait="wait" :checkFull="checkFull" :checkName="checkName" :letPlay="letPlay" :color="color" :myAvatar="myAvatar" :f="f" :c="c" :selectFace="selectFace" :selectColor="selectColor"> </form-befor-play>
    <countdown-to-play :waitingTime="waitingTime" :checkName="checkName"></countdown-to-play>
  </div>
</template>

<script>
import BackgroundAndLogo from './BackgroundAndLogo'
import ServerFull from './ServerFull'
import FormBeforPlay from './FormBeforPlay'
import CountdownToPlay from './CountdownToPlay'

export default {
  props: ['wait', 'checkFull', 'checkName', 'letPlay', 'color', 'myAvatar', 'f', 'c', 'selectFace', 'selectColor', 'waitingTime'],
  components: {
    BackgroundAndLogo,
    ServerFull,
    CountdownToPlay,
    FormBeforPlay
  }
}
</script>
