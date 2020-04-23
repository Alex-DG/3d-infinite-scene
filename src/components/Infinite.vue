<template>
  <canvas id="container" />
</template>

<script>
import * as THREE from 'three'

var canvas = null

var CAMERA_Z = 500

var scene = new THREE.Scene()
// var renderer = new THREE.WebGLRenderer({ canvas })
var renderer = null
var camera = new THREE.PerspectiveCamera(45, 2, 0.1, CAMERA_Z)

var loopSectionPosition = 0

var mesh = null
var dummy = new THREE.Object3D()
var sectionWidth = 200
var CUBE = 40

var goZoom = false

function initThree() {
  canvas = document.getElementById('container')
  renderer = new THREE.WebGLRenderer({ canvas })
  renderer.setSize(window.innerWidth, canvas.clientHeight)

  // Creating the scene, renderer, camera
  renderer.setPixelRatio(window.devicePixelRatio + 1 || 1)
  renderer.setClearColor(0x161216)

  camera.position.y = 0
  camera.position.z = CAMERA_Z

  // document.body.appendChild(renderer.domElement)
}

function resize() {
  renderer.height = canvas.clientHeight
  renderer.width = window.innerWidth

  renderer.setSize(renderer.width, renderer.height)

  camera.aspect = renderer.width / renderer.height

  camera.updateProjectionMatrix()
}

function addInstancedMesh() {
  // An InstanceMesh of 4 Cubes
  mesh = new THREE.InstancedMesh(
    new THREE.BoxBufferGeometry(CUBE, CUBE, CUBE),
    new THREE.MeshNormalMaterial(),
    8
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
  // Zoom In
  if (goZoom && camera.position.z > 100) {
    camera.position.z = lerp(camera.position.z, 100, 0.014)
  }

  // Zoom Out
  if (camera.position.z < CAMERA_Z && !goZoom) {
    camera.position.z = lerp(camera.position.z, CAMERA_Z, 0.014)
  }

  // Move the camera
  camera.position.x += 2.0

  loop()

  renderer.render(scene, camera)
}

// Transform.position, wantedPosition, Time.deltaTime
function lerp(a, b, t) {
  return (1 - t) * a + t * b
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

  document.body.addEventListener(
    'click',
    () => {
      goZoom = !goZoom
    },
    { passive: true }
  )

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
canvas {
  /* display: block; */
  width: 100%;
  height: 450px;
}
</style>
