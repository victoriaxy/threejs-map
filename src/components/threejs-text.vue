<template>
    <div ref="threeLineRef" class="threejs-example"></div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { Scene, PerspectiveCamera, WebGLRenderer, MeshPhongMaterial, Mesh } from 'three';
import { TextGeometry } from 'three/examples/jsm/geometries/TextGeometry';
import { FontLoader } from 'three/examples/jsm/loaders/FontLoader';

const threeLineRef = ref();

const scene = new Scene();
const camera = new PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 500);
// 
camera.position.set(0,0,100);
// 
camera.lookAt( 0, 0, 0 );

const renderer = new WebGLRenderer();

// 线的材质，LineDashedMaterial
const materials = new MeshPhongMaterial( { color: 0xffffff } );
const loader = new FontLoader();

loader.load('three/examples/fonts/helvetiker_bold.typeface.json', function ( font ) {
    const textGeo = new TextGeometry('Hello, Three.js', {
        font: font,
        size: 80,
        height: 5,
        curveSegments: 12,
        bevelEnabled: true,
        bevelThickness: 10,
        bevelSize: 8,
        bevelSegments: 5
    });
    textGeo.computeBoundingBox();

    const centerOffset = - 0.5 * ( textGeo.boundingBox.max.x - textGeo.boundingBox.min.x );
    const textMesh1 = new Mesh( textGeo, materials );
    textMesh1.position.x = centerOffset;
    scene.add(textMesh1);
});

function render() {
    renderer.render(scene, camera);
}

onMounted(() => {
    renderer.setSize(threeLineRef.value.clientWidth, window.innerHeight);
    threeLineRef.value.appendChild(renderer.domElement);
    render();
})

</script>