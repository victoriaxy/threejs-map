<template>
    <div ref="threeFirstRef" class="threejs-example"></div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import * as THREE from 'three';
import WebGL from '../utils/web-gl';

const threeFirstRef = ref();
/**
 * 1、场景
 */
const scene = new THREE.Scene();

/**
 * 2、摄像机,PerspectiveCamera透视摄像机
 * param1: 视野角度（FOV）
 * param2: 长宽比（aspect ratio）
 * param3: 近截面（near）
 * param4: 远截面（far）
 */
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

/**
 * 3、渲染器,可根据浏览器对WebGL的支持力度选择不同的渲染器
 */
const renderer = new THREE.WebGLRenderer();
// 设置渲染器的尺寸
// renderer.setSize(width, height);

/**
 * 4、为场景添加实物
 */
// 创建一个盒子 
const geometry = new THREE.BoxGeometry(2,2,2);
// 装饰盒子
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 } );
// 创建一个网格Mesh，存放装饰过的盒子
const cube= new THREE.Mesh(geometry, material);
scene.add(cube);
// 摄像机偏移位置，避免摄像机和盒子在一起【默认初始渲染坐标是（0,0,0）】
camera.position.z = 5;

/**
 * 5、渲染场景，执行这一步页面中方可看到效果
 */
 function animate() {
    // 默认刷新频率： 60次/秒，与屏幕的渲染帧有关。
	requestAnimationFrame( animate );
    // 设置旋转动效
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    // 渲染场景和摄像机
	renderer.render( scene, camera );
}
onMounted(() => {
    if (WebGL.isWebGLAvailable()) {
        // 设置渲染器的尺寸
        renderer.setSize(threeFirstRef.value.clientWidth, window.innerHeight);
        threeFirstRef.value.appendChild(renderer.domElement)
        animate();
    } else {
        const warning = WebGL.getWebGLErrorMessage();
        threeFirstRef.value.appendChild(warning);
    }
   
})
</script>
