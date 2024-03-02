<script setup>
import { TresCanvas } from '@tresjs/core';
import { OrbitControls } from '@tresjs/cientos';
import {
  Mesh,
  BoxGeometry,
  MeshBasicMaterial,
  Color,
  PerspectiveCamera,
  TextureLoader,
  PlaneGeometry,
} from "three";
import { ref } from 'vue';

import { useTexture, useRenderLoop } from '@tresjs/core';

const pbrTexture = await useTexture({
  map: '/images/person.png',
});

const secondGeometry = new BoxGeometry(1, 1, 1);
const secondMaterial = new MeshBasicMaterial({ color: 0x00ff00 });
const qq = new MeshBasicMaterial({map: pbrTexture.map})
const secondMesh = new Mesh(secondGeometry, qq);

secondMesh.position.set(2, 0, 0);




</script>

<template>
  <TresCanvas
      clear-color="#82DBC5"
      shadows
      alpha
      window-size
      power-preference="high-performance"

  >
    <OrbitControls />
    <TresPerspectiveCamera
        :position="[1, 2, 5]"
        :fov="45"
        :aspect="1"
        :near="0.1"
        :far="1000"
    />
    <primitive :object="secondMesh" />
      <TresMesh :scale="1" cast-shadow>
        <TresBoxGeometry :args="[1, 1, 0.1]" />
        <TresMeshStandardMaterial
            v-bind="pbrTexture"
            displacement-scale="0.2"
        />
      </TresMesh>
    <TresDirectionalLight :position="[0, 2, 4]" :intensity="2" cast-shadow />
  </TresCanvas>

</template>
