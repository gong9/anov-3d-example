<script setup lang='ts'>
import { onMounted, ref } from 'vue'
import { BoxGeometry, Mesh, MeshBasicMaterial, Scene, createLabel, dom } from '@anov/3d'

const divRef = ref(null)

onMounted(() => {
  const scene = new Scene({
    orbitControls: true,
    css2DRenderer: true,
  })

  const geometry = new BoxGeometry(2, 2, 2)
  const material = new MeshBasicMaterial({ color: 0x00FF00 })
  const box = new Mesh(geometry, material)

  box.addNatureEventListener('pointermove', (object3D) => {
    (object3D.material as any).color.set(0xFF0000)
  })
  box.addNatureEventListener('pointerleave', (object3D) => {
    (object3D.material as any).color.set(0x00FF00)
  })

  const labelObject2d = createLabel(dom.createElement('div', {
    textContent: '测试',
    style: {
      color: 'red',
      fontSize: '50px',
    },
  }))

  labelObject2d.position.set(0, 3, 0)

  box.add(labelObject2d)
  scene.add(box)

  scene.render(divRef.value!)
  scene.startFrameAnimate()
})
</script>

<template>
  <div ref="divRef" class="canvas" />
</template>

<style scoped lang='scss'>
.canvas {
  width: 100vw;
  height: 100vh;
}
</style>
