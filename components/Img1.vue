<template>
  <TresCanvas window-size clear-color="#82dbc5">
    <TresPerspectiveCamera :args="[45, 1, 0.1, 1000]" />
    <OrbitControls />
    <Suspense>
      <TresMesh>
        <TresSphereGeometry :args="[1, 32, 32]" />
        <TresMeshStandardMaterial :map="loadedTexture" />
      </TresMesh>
    </Suspense>
    <TresDirectionalLight :position="[0, 2, 4]" :intensity="2" cast-shadow />
  </TresCanvas>
</template>

<script>
import { ref, onMounted } from "vue";
import { useLoader } from "@tresjs/core";
import { TextureLoader } from "three";

export default {
  setup() {
    const loadedTexture = ref(null);

    onMounted(async () => {
      const texture = await useLoader(
        TextureLoader,
        "https://raw.githubusercontent.com/Tresjs/assets/main/textures/black-rock/Rock035_2K_Displacement.jpg",
      );
      console.log(texture);
      loadedTexture.value = texture;
    });

    return {
      loadedTexture,
    };
  },
};
</script>
