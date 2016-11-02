<template>
  <div v-show="!checkName" class="popUpName">
    <h4 style="color: #00CC00; font-size: 2em;">
        Ready in {{waitingTime}}
      </h4>
  </div>
</template>

<script>
export default {
  props: ['checkName', 'waitingTime']
}
</script>
