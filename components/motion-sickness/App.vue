<script setup>
import { VPButton } from 'vitepress/theme';

// todo: Implement a proper handling by recycling oscillator, and providing
//       pause support.

// todo: Add a gentle fade in and fade out (especially fade out to avoid
//       clipping)

const context = new AudioContext();
let oscillator = null;

function start() {
    if (oscillator) {
        // Debounce
        return;
    }

    oscillator = context.createOscillator();
    oscillator.frequency.value = 100;
    oscillator.connect(context.destination);

    oscillator.start();
    oscillator.stop(context.currentTime + 5);

    oscillator.addEventListener('ended', () => {
        oscillator.disconnect();
        oscillator = null;
    });
}

function stop() {
    if (oscillator) {
        oscillator.stop();
    }
}
</script>

<template>
    <div class="button-row">
        <VPButton theme="alt" text="▶️ Start" @click="start" />
        <VPButton theme="alt" text="⏹️ Stop" @click="stop" />
    </div>
</template>

<style lang="css" scoped>
.button-row {
    display: flex;
    gap: 0.5rem;
}
</style>