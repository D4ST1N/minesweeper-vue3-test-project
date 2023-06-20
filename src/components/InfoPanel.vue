<script setup lang="ts">
import { ref, computed, watch } from "vue";
import { storeToRefs } from "pinia";
import { useGameStore } from "../stores/game";
import { GameCondition } from "@/types/gameTypes";

const store = useGameStore();
const timer = ref(0);
const { status } = storeToRefs(store);
let timerId: number | null = null;

const time = computed(
  () =>
    `${Math.floor(timer.value / 60)}:${(timer.value % 60).toLocaleString("en-US", {
      minimumIntegerDigits: 2,
    })}`,
);

watch(status, (newStatus: GameCondition) => {
  if (newStatus === GameCondition.Playing) {
    startTimer();
  }

  if (newStatus === GameCondition.Victory || newStatus === GameCondition.Defeat) {
    stopTimer();
  }
});

function startTimer() {
  timerId = setInterval(() => {
    timer.value += 1;
  }, 1000);
}

function stopTimer() {
  if (!timerId) return;

  clearInterval(timerId);
}
</script>

<template>
  <div :class="$style.panelWrapper">
    <div :class="$style.bombsCount">
      <v-icon icon="mdi-mine" :size="28" />
      {{ store.bombsCount }}
    </div>
    <div :class="$style.separator">|</div>
    <div :class="$style.timer">
      {{ time }}
      <v-icon icon="mdi-timer" :size="28" />
    </div>
  </div>
</template>

<style lang="scss" module>
.panelWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  color: #efefef;
  gap: 12px;
}

.bombsCount {
  font-size: 36px;
  min-width: 120px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 16px;
}

.separator {
  font-size: 40px;
  font-weight: bold;
}

.timer {
  font-size: 36px;
  min-width: 120px;
  text-align: left;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
