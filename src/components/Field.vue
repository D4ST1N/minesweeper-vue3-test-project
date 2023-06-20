<script setup lang="ts">
import { useGameStore } from "../stores/game";
import FieldCell from "./FieldCell.vue";
import type { FieldCellCoordinates } from "@/types/gameTypes";

const store = useGameStore();

function handleLeftClick(x: number, y: number) {
  const coordinates: FieldCellCoordinates = { x, y };

  // We want the player's first click to always be on a blank cell, so we populate field after first click
  if (!store.bombsPlanted) {
    store.populateGame(coordinates);
  }

  store.openCell(coordinates);
}

function handleMiddleClick(x: number, y: number) {
  const coordinates: FieldCellCoordinates = { x, y };

  store.highlightNeighbors(coordinates);
}

function handleRightClick(x: number, y: number) {
  const coordinates: FieldCellCoordinates = { x, y };

  store.toggleLabel(coordinates);
}
</script>

<template>
  <div :class="$style.field">
    <div v-for="(gameFieldRow, index) in store.field" :key="index" :class="$style.fieldRow">
      <FieldCell
        v-for="(fieldCell, fieldCellIndex) in gameFieldRow"
        :key="fieldCellIndex"
        :field-cell="fieldCell"
        @left-click="() => handleLeftClick(index, fieldCellIndex)"
        @middle-click="() => handleMiddleClick(index, fieldCellIndex)"
        @right-click="() => handleRightClick(index, fieldCellIndex)"
      />
    </div>
  </div>
</template>

<style lang="scss" module>
.field {
  display: flex;
}

.fieldRow {
  display: flex;
  flex-direction: column;
}
</style>
