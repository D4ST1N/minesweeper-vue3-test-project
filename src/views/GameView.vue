<script setup lang="ts">
import { useGameStore } from "@/stores/game";
import { FieldSize, GameCondition } from "@/types/gameTypes";
import Field from "@/components/Field.vue";
import InfoPanel from "@/components/InfoPanel.vue";

const store = useGameStore();

function startGame(size: FieldSize) {
  store.startNewGame(size);
}

function endGame() {
  store.endGame();
}
</script>

<template>
  <div>
    <div v-if="store.field">
      <InfoPanel />
      <Field />
    </div>
    <div v-else :class="$style.buttons">
      <v-btn @click="() => startGame(FieldSize.Small)">Small</v-btn>
      <v-btn @click="() => startGame(FieldSize.Medium)">Medium</v-btn>
      <v-btn @click="() => startGame(FieldSize.Expert)">Expert</v-btn>
    </div>
  </div>
  <div v-if="store.status === GameCondition.Victory" :class="$style.alertContainer">
    <div :class="$style.alertWrapper">
      <v-alert type="success" title="You Won!">
        <v-btn @click="endGame">Start a New Game</v-btn>
      </v-alert>
    </div>
  </div>
  <div v-if="store.status === GameCondition.Defeat" :class="$style.alertContainer">
    <div :class="$style.alertWrapper">
      <v-alert type="error" title="Defeated!">
        <v-btn @click="endGame">Start a New Game</v-btn>
      </v-alert>
    </div>
  </div>
</template>

<style lang="scss" module>
.buttons {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.alertContainer {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.67);
  display: flex;
  align-items: center;
  justify-content: center;
}

.alertWrapper {
  width: 300px;
}
</style>
