<template>
  <div class="score-container">
    <div class="score">
      <label for="">Player</label>
      <span>{{winCount}}</span>
    </div>
    <div class="control">x</div>
    <div class="score">
      <span>{{loseCount}}</span>
      <label for="">Computer</label>
    </div>
  </div>
</template>
<script>
export default {
  props: ['winCount', 'loseCount']
}
</script>
