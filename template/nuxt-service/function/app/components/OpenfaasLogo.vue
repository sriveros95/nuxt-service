<template>
  <img
    class="OpenFaasLogo"
    alt="OpenFaaS Logo"
    src="/OpenFaas_whale.png"
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
