<script setup>
import { ref, watchEffect } from 'vue'

const slides = ref([
  { id: 'slide1', text: 'Slide 1' },
  { id: 'slide2', text: 'Slide 2' },
  { id: 'slide3', text: 'Slide 3' }
])

const content = ref(null)
const active = ref(0)
const position = ref(0)

watchEffect(() => {
  const { width } = content.value.getBoundingClientRect()
  position.value = -(active.value * width)
},
  {
    flush: 'post'
  })

function prev() {
  if (active.value > 0) {
    active.value -= 1
  }
}
function next() {
  if (active.value < slides.value.length - 1) {
    active.value += 1
  }
}
</script>

<template>
  <section class="container">
    <div ref="content" class="content" :style="{ transform: `translateX(${position}px)` }">
      <div v-for="slide in slides" :key="slide.id" class="item">{{ slide.text }}</div>
    </div>
    <nav class="nav">
      <button @click="prev">Previous</button>
      <button @click="next">Next</button>
    </nav>
  </section>
</template>

<style>
.container {
  overflow: hidden;
}
.content {
  display: flex;
  transition: transform 0.3s ease;
}
.item {
  width: 80%;
  flex-shrink: 0;
  margin: 0 10%;
  padding: 10rem 0;
  background-color: #eee;
  border-radius: 4px;
  text-align: center;
}
.nav {
  display: flex;
  justify-content: space-between;
  margin: 1rem auto;
  width: 80%;
}
</style>
