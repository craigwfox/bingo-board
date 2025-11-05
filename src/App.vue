<template>
  <div id="app">
    <h1>
      <img src="/yodel-trey-head.png" alt="" />
      <span>Trey's Bingo <em>Blue</em>nanza</span>
    </h1>
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

h1 {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;

  color: #16466e;
  font-size: 5rem;

  img {
    max-width: 50px;
  }

  em {
    color: #2475b7;
  }
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
