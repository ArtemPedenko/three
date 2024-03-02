<template>
  <div
    style="position: relative; height: 100vh; width: 100vw"
    @wheel="handleScroll"
  >
    <TresCanvas :ref="canvasRef" window-size clear-color="#82dbc5">
      <!--      <OrbitControls />-->
      <TresPerspectiveCamera
        :position="[cameraPosition.x, cameraPosition.y, cameraPosition.z]"
        :fov="45"
        :aspect="1"
        :near="0.1"
        :far="1000"
      />
      <primitive :object="myText" />
      <primitive
        :object="meshWithMaterial"
        @click="
          (intersection, pointerEvent) =>
            handleMeshClick(intersection, pointerEvent)
        "
      />
      <primitive :object="secondMesh" />
      <primitive :object="thirdMesh" />
      <primitive :object="fourthMesh" />
    </TresCanvas>
    <div style="position: absolute; top: 10px; left: 10px; z-index: 100">
      <input />
    </div>
  </div>
</template>

<script setup>
import {
  Mesh,
  BoxGeometry,
  MeshBasicMaterial,
  Color,
  PerspectiveCamera,
  TextureLoader,
  PlaneGeometry,
  DoubleSide,
} from "three";
import { Text } from "troika-three-text";
import { reactive, ref } from "vue";
import { useTexture } from "@tresjs/core";
import { OrbitControls } from "@tresjs/cientos";

const pbrTexture = await useTexture({
  map: "/images/person.png",
});

const canvasRef = ref(null);

const cameraPosition = reactive({ x: 0, y: 0, z: 5 });

const geometry = new BoxGeometry(2, 1, 1);
const material = new MeshBasicMaterial({ color: 0xdc143c });
const newColor = new Color(0x0000ff);
const meshWithMaterial = new Mesh(geometry, material);

const secondGeometry = new PlaneGeometry(1, 1);
const secondMaterial = new MeshBasicMaterial({
  map: pbrTexture.map,
  side: DoubleSide,
  alphaTest: 1,
});
const secondMesh = new Mesh(secondGeometry, secondMaterial);

secondMesh.position.set(2, 0, 0);

const thirdGeometry = new BoxGeometry(1, 1, 1);
const thirdMaterial = new MeshBasicMaterial({ color: 0xff0000 });
const thirdMesh = new Mesh(thirdGeometry, thirdMaterial);

thirdMesh.position.set(0, -2, 0);

const fourthGeometry = new BoxGeometry(1, 1, 1);
const fourthMaterial = new MeshBasicMaterial({ color: 0xffff00 });
const fourthMesh = new Mesh(fourthGeometry, fourthMaterial);

fourthMesh.position.set(0, -4, 0);

const myText = new Text();
myText.text = "Hello world!";
myText.fontSize = 0.2;
myText.position.z = -2;
myText.color = 0x9966ff;
myText.position.set(0, 0, 1);

///////
const handleMeshClick = (intersection, pointerEvent) => {
  material.color = newColor;
};

const handleScroll = (event) => {
  const deltaY = event.deltaY;
  if (cameraPosition.y - deltaY * 0.01 < 1) {
    cameraPosition.y -= deltaY * 0.01;
  }
};
</script>
