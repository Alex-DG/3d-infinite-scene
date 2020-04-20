<template>
  <div id="container"></div>
</template>

<script>
import * as THREE from 'three'

var scene = new THREE.Scene()
var renderer = new THREE.WebGLRenderer()
var camera = new THREE.PerspectiveCamera(45, 0, 1, 2000)

var loopSectionPosition = 0

var mesh = null
var dummy = new THREE.Object3D()
var sectionWidth = 200

function initThree() {
  // Creating the scene, renderer, camera

  renderer.setPixelRatio(window.devicePixelRatio || 1)
  renderer.setClearColor(0x161216)

  camera.position.y = 0
  camera.position.z = 2000

  document.body.appendChild(renderer.domElement)
}

function resize() {
  renderer.height = window.innerHeight
  renderer.width = window.innerWidth
  renderer.setSize(renderer.width, renderer.height)

  camera.aspect = renderer.width / renderer.height

  camera.updateProjectionMatrix()
}

function addInstancedMesh() {
  // An InstanceMesh of 4 Cubes
  mesh = new THREE.InstancedMesh(
    new THREE.BoxBufferGeometry(50, 50, 50),
    new THREE.MeshNormalMaterial(),
    4
  )

  scene.add(mesh)

  setInstanceMeshPositions(mesh, 0)
}

function setInstanceMeshPositions(mesh, section) {
  for (var i = 0; i < mesh.count; i++) {
    // We add 200 units of distance (the width of the section) between each.

    var xStaticPositon = -sectionWidth * (i - 1)
    var xSectionPosition = sectionWidth * section
    var x = xStaticPositon + xSectionPosition

    dummy.position.set(x, 0, 0)
    dummy.updateMatrix()

    mesh.setMatrixAt(i, dummy.matrix)
  }

  mesh.instanceMatrix.needsUpdate = true
}

function loop() {
  var distance = Math.round(camera.position.x / sectionWidth)
  if (distance !== loopSectionPosition) {
    loopSectionPosition = distance
    setInstanceMeshPositions(mesh, loopSectionPosition)
  }
}

function render() {
  // Move the camera
  camera.position.x += 5.0

  loop()

  renderer.render(scene, camera)
}

function animate() {
  // Render the 3D scene
  render()
  requestAnimationFrame(animate)
}

function init() {
  initThree()
  resize()

  window.addEventListener('resize', resize, {
    passive: true
  })

  addInstancedMesh()
  animate()
}

export default {
  name: 'Infinite',
  methods: {
    init,
    resize
  },
  mounted() {
    init()
  }
}
</script>

<style scoped>
/* #container {
  height: 300px;
} */
</style>
