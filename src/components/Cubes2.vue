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

      <primitive
        :object="secondMesh"
        @click="
          (intersection, pointerEvent) =>
            handleMeshClick(intersection, pointerEvent)
        "
      />
      <primitive :object="myText" />
      <primitive :object="thirdMesh" />
    </TresCanvas>
    <div
      v-if="modal"
      style="
        position: absolute;
        top: 0;
        left: 0;
        z-index: 100;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        width: 100vw;
      "
      @click="handleMeshClick"
    >
      <div
        style="
          padding: 100px;
          display: flex;
          justify-content: center;
          align-items: center;
          background-color: bisque;
        "
      >
        <input />
      </div>
    </div>
  </div>
</template>

<script setup>
import {
  Mesh,
  MeshBasicMaterial,
  PlaneGeometry,
  DoubleSide,
  BoxGeometry,
} from "three";
import { reactive, ref } from "vue";
import { useTexture } from "@tresjs/core";
import { Text } from "troika-three-text";

const pbrTexture = await useTexture({
  map: "/images/person.png",
});
const modal = ref(false);
const canvasRef = ref(null);

const cameraPosition = reactive({ x: 0, y: 0, z: 4 });

const secondGeometry = new PlaneGeometry(1, 1);
const secondMaterial = new MeshBasicMaterial({
  map: pbrTexture.map,
  side: DoubleSide,
  transparent: true,
  opacity: calculateOpacity(19 - Math.abs(cameraPosition.z)),
});
const secondMesh = new Mesh(secondGeometry, secondMaterial);
secondMesh.position.set(0, 0, -15);

const thirdGeometry = new BoxGeometry(1, 1, 1);
const thirdMaterial = new MeshBasicMaterial({
  map: pbrTexture.map,
  transparent: true,
  opacity: calculateOpacity(19 - Math.abs(cameraPosition.z)),
});
const thirdMesh = new Mesh(thirdGeometry, thirdMaterial);

thirdMesh.position.set(0, 0, -20);

const myText = new Text();
myText.text = "Профит Лига";
myText.fontSize = 0.5;
myText.color = 0x8b0000;
myText.position.set(-1.5, 0.5, -0.5);

function calculateOpacity(distance) {
  let distance1 = 10;
  let opacity1 = 1;
  let distance2 = 15;
  let opacity2 = 0;
  let opacity =
    opacity1 +
    ((opacity2 - opacity1) * (distance - distance1)) / (distance2 - distance1);
  opacity = Math.max(0, Math.min(1, opacity));
  return opacity;
}

const handleScroll = (event) => {
  const deltaY = event.deltaY;
  //console.log(cameraPosition.z);
  if (cameraPosition.z + deltaY * 0.01 < 6) {
    cameraPosition.z += deltaY * 0.01;
  }
  secondMaterial.opacity = calculateOpacity(19 - Math.abs(cameraPosition.z));
  thirdMaterial.opacity = calculateOpacity(19 - Math.abs(cameraPosition.z));

  //  console.log("cameraPosition.z", cameraPosition.z);
  console.log(calculateOpacity(19 - Math.abs(cameraPosition.z)));
};
const handleMeshClick = (intersection, pointerEvent) => {
  modal.value = !modal.value;
};
</script>
