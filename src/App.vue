<script setup lang="ts">
import { ref, onMounted } from "vue";

const canvas = ref<HTMLCanvasElement | null>(null);
var count = ref(0);
var starting = ref(true);
onMounted(() => {
  if (canvas.value != null) {
    canvas.value.width = window.innerWidth;
    canvas.value.height = window.innerHeight;
  }
});

const addPoint = (ev: MouseEvent) => {
  if (canvas.value != null) {
    const rect = canvas.value.getBoundingClientRect();
    const px = ev.clientX - rect.left;
    const py = ev.clientY - rect.y;
    const ctx = canvas.value.getContext("2d");
    if (ctx != null) {
      if (starting.value) {
        ctx.beginPath();
        ctx.arc(px, py, 5, 0, 2 * Math.PI);
        ctx.moveTo(px, py);
        console.log("begin");
        starting.value = false;
        ctx.fill();
      } else {
        console.log("line");
        ctx.arc(px, py, 5, 0, 2 * Math.PI);
        ctx.lineTo(px, py);
        ctx.stroke();
        ++count.value;
      }
      if (count.value >= 3) {
        ctx.closePath();
        ctx.stroke();
        starting.value = true;
        count.value = 0;
      }
    }
  }
};
</script>
<template>
  <canvas @mousedown="addPoint" id="canvas" ref="canvas"></canvas>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#canvas {
  border: 2px solid black;
}
</style>
