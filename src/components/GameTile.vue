<template>
  <div :class="['game-tile',
  {'isCorrect': isCorrect},
  {'isClose': isClose},
  {'isWrong': isWrong}
  ]">
    {{ value }}
  </div>
</template>

<script>

export default {
  name: 'GameTile',
  props: {
    value: String,
    isCorrect: Boolean,
    isClose: Boolean,
    isWrong: Boolean,
  }
}
</script>

<style scoped lang="scss">
  .game-tile{
    border: solid 1px #a3a3a3;
    border-radius: 5px;
    padding: 20px;
    height: 30px;
    width: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 40px;
    text-transform: uppercase;
    &.isCorrect {
      border-color: #4cac4c;
      background-color: #4cac4c ;
      color: #fff;
    }
    &.isClose {
      border-color: #f0c246;
      background-color: #f0c246 ;
      color: #fff;
    }
    &.isWrong {
      border-color: #848585;
      background-color: #848585 ;
      color: #fff;
    }
  }
</style>
