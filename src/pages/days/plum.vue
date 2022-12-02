<script setup lang="ts">
defineOptions({
  name: 'plum',
})
const el = $ref<HTMLCanvasElement>()
const ctx = $computed(() => el!.getContext('2d')!)

const WIDTH = 600
const HEIGHT = 600

interface Point {
  x: number
  y: number
}

interface Branch {
  start: Point
  length: number
  angel: number
}

const pendingTasks: Function[] = []

function flushTasks() {
  const tasks = [...pendingTasks]
  pendingTasks.length = 0
  tasks.forEach(fn => fn())
}

function startAnimation() {
  requestAnimationFrame(() => {
    flushTasks()
    startAnimation()
  })
}

function init() {
  ctx.strokeStyle = 'rgba(0, 0, 0, 0.5)'
  const branch: Branch = {
    start: { x: WIDTH / 2, y: HEIGHT },
    length: 15,
    angel: -Math.PI / 2,
  }
  step(branch)
  startAnimation()
}

function lineTo(p1: Point, p2: Point) {
  ctx.beginPath()
  ctx.moveTo(p1.x, p1.y)
  ctx.lineTo(p2.x, p2.y)
  ctx.stroke()
}

function getEndPoint(b: Branch): Point {
  return {
    x: b.start.x + b.length * Math.cos(b.angel),
    y: b.start.y + b.length * Math.sin(b.angel)
  }
};

function drawBranch(b: Branch) {
  lineTo(b.start, getEndPoint(b));
}

function step(b: Branch, depth = 1) {
  const end = getEndPoint(b);
  drawBranch(b);
  if (depth < 8 || Math.random() < 0.5) {
    pendingTasks.push(() => {
      step({
        start: end,
        length: b.length,
        angel: b.angel - 0.2,
      }, depth + 1)
    })
  }
  if (depth < 8 || Math.random() < 0.5) {
    pendingTasks.push(() => {
      step({
        start: end,
        length: b.length,
        angel: b.angel + 0.2,
      }, depth + 1)
    })
  }
}

onMounted(() => {
  init();
})

</script>

<template>
  <canvas ref="el" width="600" height="600" border></canvas>
</template>
