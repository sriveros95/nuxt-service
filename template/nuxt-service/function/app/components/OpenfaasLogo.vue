<template>
  <img
    class="OpenFaasLogo"
    alt="Vuetify Logo"
    src="/OpenFaaS_logo_stacked_opaque.png"
  >
</template>

<style>
.OpenFaasLogo {
  width: 180px;
  transform: rotateY(420deg);
  animation: turn 4.2s ease-out forwards 1s;
}

@keyframes turn {
  100% {
    transform: rotateY(0deg);
  }
}
</style>
