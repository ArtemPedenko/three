<template>
  <TresCanvas window-size clear-color="#82dbc5">
    <OrbitControls />
    <TresPerspectiveCamera :args="[45, 1, 0.1, 1000]" />
    <!--    <TresMesh-->
    <!--      ref="boxRef"-->
    <!--      @click="-->
    <!--        (intersection, pointerEvent) =>-->
    <!--          console.log('click', intersection, pointerEvent)-->
    <!--      "-->
    <!--    >-->
    <!--      <TresBoxGeometry />-->
    <!--      <TresMeshNormalMaterial />-->
    <!--    </TresMesh>-->
    <primitive
      :object="meshWithMaterial"
      @click="
        (intersection, pointerEvent) =>
          handleMeshClick(intersection, pointerEvent)
      "
    />
    <primitive :object="secondMesh" />
  </TresCanvas>
</template>
<script setup>
import {
  Mesh,
  BoxGeometry,
  MeshBasicMaterial,
  SRGBColorSpace,
  Color,
} from "three";

// Create a box geometry and a basic material
const geometry = new BoxGeometry(1, 1, 1);
const material = new MeshBasicMaterial({ color: 0xdc143c });
const newColor = new Color(0x0000ff);

const secondGeometry = new BoxGeometry(1, 1, 1);
const secondMaterial = new MeshBasicMaterial({ color: 0x00ff00 });
const secondMesh = new Mesh(secondGeometry, secondMaterial);

secondMesh.position.set(2, 0, 0);
const meshWithMaterial = new Mesh(geometry, material);
const handleMeshClick = (intersection, pointerEvent) => {
  // console.log('click', intersection, pointerEvent);
  material.color = newColor;
};
</script>
