<template>
    <div ref="threeLineRef" class="threejs-example"></div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { Scene, PerspectiveCamera, WebGLRenderer, LineBasicMaterial, Vector3, BufferGeometry, Line } from 'three';

const threeLineRef = ref();

const scene = new Scene();
const camera = new PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 500);
// 
camera.position.set(0,0,100);
// 
camera.lookAt( 0, 0, 0 );

const renderer = new WebGLRenderer();

// 线的材质，LineDashedMaterial
const material = new LineBasicMaterial({color: 0x0000ff });
// 创建一些立体的点
const points = [];
points.push( new Vector3( - 10, 0, 0 ) );
points.push( new Vector3( 0, 10, 0 ) );
points.push( new Vector3( 10, 0, 0 ) );
const geometry = new BufferGeometry().setFromPoints(points);
// 将材质和点连接为线
const line = new Line(geometry, material);
scene.add(line);

function render() {
    renderer.render(scene, camera);
}

onMounted(() => {
    renderer.setSize(threeLineRef.value.clientWidth, window.innerHeight);
    threeLineRef.value.appendChild(renderer.domElement);
    render();
})

</script>