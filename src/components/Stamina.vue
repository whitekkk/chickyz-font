<template>
  <div style="text-align: center;">
    <h5 class="score" style="left:290px; bottom: 0%;">
          Stamina :
        </h5>
        <h5 class="score" style="left:360px; bottom: 0%;">
          <progress v-model="countStep" max="100"></progress>
          {{countStep}}
        </h5>
  </div>
</template>

<script>
export default {
  props: ['countStep']
}
</script>
