<template>
  <header>
    <h1>Travel Tips <img class="airplane" src="/src/assets/airplane.png" alt="Airplane"></h1>
    <p>Busque um roteiro de viagem personalizado para sua <span class="underline">viagem</span>!</p>
  </header>
</template>

<script>
export default {}
</script>

<style scoped>
header {
  background-color: #f5f5f57b;
  padding: 20px 10px;
  text-align: center;
  margin-bottom: 2em;
}

h1 {
  color: #213547;
  font-size: 3.2em;
  line-height: 1.1;
  position: relative;
}

.airplane {
  position: absolute;
  bottom: 39%;
  /*right: 41%;*/
  width: 44px;
}

p {
  font-size: 1.2em;
  color: #213547;
  margin-top: 0.4em;
}

.underline {
  text-decoration: underline;
  font-weight: 600;
}
</style>