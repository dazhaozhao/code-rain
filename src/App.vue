<script setup lang="ts">
const str = "console.log('hello world')"

const el = ref<HTMLCanvasElement>()

const init = () => {
  const ctx = el.value!.getContext('2d')!
  el.value!.width = window.innerWidth
  el.value!.height = window.innerHeight
  const width = el.value!.width
  const height = el.value!.height
  const columns = width / 14
  const drops = Array.from({ length: columns }, () => 0)
  const draw = () => {
    ctx.fillStyle = 'rgba(0,0,0,0.05)'
    ctx.fillRect(0, 0, width, height)
    ctx.fillStyle = '#0f0'
    ctx.font = '14px arial'
    drops.forEach((drop, i) => {
      const text = str[Math.floor(Math.random() * str.length)]
      ctx.fillText(text, i * 14, drop * 14)
      if (drop * 14 > height && Math.random() > 0.975) drops[i] = 0
      drops[i]++
    })
  }
  return draw
}
const resizeCanvas = () => {
  window.addEventListener('resize', () => {
    el.value!.width = window.innerWidth
    el.value!.height = window.innerHeight
  })
}
onMounted(() => {
  resizeCanvas()
  const draw = init()
  setInterval(draw, 33)
})
</script>

<template>
  <main>
    <canvas ref="el" class="w-full h-full"></canvas>
  </main>
</template>
