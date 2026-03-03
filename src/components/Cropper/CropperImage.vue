<template>
  <div class="cropper-image" @mousedown="onDragStart" @mouseup="onDragStop" ref="cropper">
    <img :src="image" alt="" />
    <div class="cropper-image__overlay"></div>

    <div class="cropper-image__controls">
      <div class="cropper-image__controls__item lt"></div>
      <div class="cropper-image__controls__item lb"></div>
      <div class="cropper-image__controls__item rt"></div>
      <div class="cropper-image__controls__item rb"></div>
      <div class="cropper-image__controls__wrap">
        <div class="cropper-image__controls__view">
          <img :src="image" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

type ICoords = {
  x: number | null
  y: number | null
}

const props = defineProps<{
  image: string
}>()

const startDrag = ref<ICoords>({
  x: null,
  y: null,
})
const stopDrag = ref<ICoords>({
  x: null,
  y: null,
})

const cropper = ref<HTMLDivElement>()

const dragStyles = ref({
  transform: `translate(${startDrag.value.x}px, ${startDrag.value.y}px)`,
  width: `${calculateWidthHeight(startDrag.value.x, stopDrag.value.x)}px`,
  height: `${calculateWidthHeight(startDrag.value.y, stopDrag.value.y)}px`,
})

function calculateWidthHeight(x1: number | null, x2: number | null) {
  if (x1 !== null && x2 !== null) {
    return x2 - x1
  }
  return 0
}

function onDragStart(event: MouseEvent) {
  startDrag.value.x = event.x
  startDrag.value.y = event.y
  cropper.value?.addEventListener('mousemove', onDragMove)
}
function onDragMove(event: MouseEvent) {
  stopDrag.value.x = event.x
  stopDrag.value.y = event.y
  console.log(stopDrag.value)
}
function onDragStop() {
  cropper.value?.removeEventListener('mousemove', onDragMove)
}
</script>

<style>
.cropper-image {
  width: 800px;
  height: 600px;
  overflow: hidden;
  border: 1px solid #000;
  position: relative;
}
.cropper-image img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
.cropper-image__overlay {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  display: block;
  width: 100%;
  height: 100%;
  background: #ffffff50;
}
.cropper-image__img {
  width: auto;
  height: auto;
  object-fit: contain;
}

.cropper-image__controls {
  position: absolute;
  overflow: hidden;
  width: 200px;
  height: 100px;
  left: 100px;
  top: 100px;
  border: 2px solid #000;
}
.cropper-image__controls__view {
  width: 800px;
  height: 600px;
}
.cropper-image__controls__view img {
  width: 100%;
  height: 100%;
  transform: translate(-100px, -100px);
  object-fit: contain;
}

.cropper-image__controls__item {
  position: absolute;
  background: blue;
  width: 20px;
  height: 20px;
  z-index: 10;
}
.cropper-image__controls__wrap {
  position: relative;
  width: 100%;
  height: 100%;
}
.cropper-image__controls__item.lt {
  left: 0;
  top: 0;
}
.cropper-image__controls__item.lb {
  left: 0;
  bottom: 0;
}
.cropper-image__controls__item.rt {
  right: 0;
  top: 0;
}
.cropper-image__controls__item.rb {
  right: 0;
  bottom: 0;
}
</style>
