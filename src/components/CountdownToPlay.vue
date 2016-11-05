<template>
  <div v-show="!checkName" class="popUpName">
    <h4 style="color: #FFFFFF; text-shadow: -3px 0 black, 0 3px black, 3px 0 black, 0 -3px black; font-size: 2em;">
        Ready in {{waitingTime}}
      </h4>
  </div>
</template>

<script>
export default {
  props: ['checkName', 'waitingTime']
}
</script>
