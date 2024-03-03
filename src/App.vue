<script setup>
import { TresCanvas } from "@tresjs/core";
import Cubes2 from "@/components/Cubes2.vue";
import ImgLo from "@/components/ImgLo.vue";
import CleanThree from "@/components/CleanThree.vue";
</script>

<template>
  <Suspense>
    <!--  <ImgLo />-->
    <Cubes2 />
    <!--    <CleanThree />-->
  </Suspense>
</template>

<style scoped></style>
