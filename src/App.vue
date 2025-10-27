<template>
  <div id="app">
    <h1>Bingo Board</h1>
    <BingoBoard />
    <WinnerTable />
  </div>
</template>

<script lang="ts" setup>
import BingoBoard from './components/BingoBoard.vue'
import WinnerTable from './components/WinnerTable.vue'
</script>

<style>
:root {
  --ffcond: 'Oswald', sans-serif;
  --ffbase: 'Roboto', sans-serif;
}
html {
  margin: 0;

  font-size: 10px;
  font-family: var(--ffbase);
}

body {
  margin: 0;
  padding: 2rem;

  font-size: 1.8rem;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  text-align: center;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: var(--ffcond);
}

.reset-button {
  margin-top: 2rem;
  padding: 0.5rem 1rem;

  border: 0;
  background: hsla(0, 0%, 40%, 1);

  color: #fff;
  font-size: 1.5rem;
  font-weight: 600;

  cursor: pointer;
}

.reset-button:focus,
.reset-button:hover {
  background: hsla(0, 0%, 50%, 1);
}
</style>
