<template>
  <div class="game-row">
    <GameTile
      v-for="(tile, index) in values"
      :key="index"
      v-bind="tile"
    />
  </div>
</template>

<script>
import GameTile from '@/components/GameTile.vue'

export default {
  name: 'GameRow',
    components: {
    GameTile,
  },
  props: {
    values: Array,
  },
}
</script>

<style scoped lang="scss">
.game-row{
  display: flex;
  justify-content: center;
  align-items: center;
  row-gap: 10px;
  column-gap: 10px;
  margin-bottom: 10px;
}

</style>
