<template>
  <div>
    <table>
      <tbody>
        <tr>
          <th scope="row">B</th>
          <td v-for="col in rowRange(1, 15)" :key="col">
            <button class="col-button" :id="'b' + col" :data-row="'rowb'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">I</th>
          <td v-for="col in rowRange(16, 30)" :key="col">
            <button class="col-button" :id="'i' + col" :data-row="'rowi'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">N</th>
          <td v-for="col in rowRange(31, 45)" :key="col">
            <button class="col-button" :id="'n' + col" :data-row="'rown'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">G</th>
          <td v-for="col in rowRange(46, 60)" :key="col">
            <button class="col-button" :id="'g' + col" :data-row="'rowg'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">O</th>
          <td v-for="col in rowRange(61, 75)" :key="col">
            <button class="col-button" :id="'o' + col" :data-row="'rowo'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <button class="reset-button" v-on:click="gameReset">Clear board</button>
  </div>
</template>

<script lang="ts" setup>
// ====---------------====
// 🚀 Imports
// ====---------------====
import { ref } from 'vue'

// ====---------------====
// 💾 Types & Interfaces
// ====---------------====
interface CurrentNumbers {
  [key: string]: string[]
}

// ====---------------====
// 🗄️ Props / Emits
// ====---------------====

// ====---------------====
// 🏪 Stores
// ====---------------====

// ====---------------====
// 💡 Data
// ====---------------====
const index = ref(0)
const currentNumbers = ref<CurrentNumbers>({
  rowb: [],
  rowi: [],
  rown: [],
  rowg: [],
  rowo: [],
})

// ====---------------====
// 🛠 Methods
// ====---------------====

function rowRange(rangeStart: number, rangeStop: number) {
  const rangeArry = []
  for (let i = rangeStart; i <= rangeStop; i++) {
    rangeArry.push(i)
  }
  return rangeArry
}

function highlightCol(e: Event) {
  const el = e.target as HTMLLIElement

  if (!el) return

  const x = el.getAttribute('data-row')
  const elId = el.getAttribute('id')

  if (!x || !elId) return

  if (el.classList.contains('active')) {
    const position: number | null = currentNumbers.value[x]?.indexOf(elId) ?? null
    if (!position) return

    el.classList.remove('active')
    currentNumbers.value[x]?.splice(position, 1)
  } else {
    el.classList.add('active')
    currentNumbers.value[x]?.push(elId)
  }
}

function gameReset() {
  const activeButtons = document.querySelectorAll('table td button.active')

  currentNumbers.value.rowb?.splice(0)
  currentNumbers.value.rowi?.splice(0)
  currentNumbers.value.rown?.splice(0)
  currentNumbers.value.rowg?.splice(0)
  currentNumbers.value.rowo?.splice(0)

  activeButtons.forEach((element) => {
    element.classList.remove('active')
  })
}

// ====---------------====
// 🌄 Lifecycle
// ====---------------====
</script>

<style lang="scss" scoped>
table {
  --colsize: 5vw;

  width: 100%;

  border-collapse: collapse;
  border: 0.5rem solid #ba9771;

  font-size: var(--colsize);
  font-family: var(--ffcond);
}

table th,
table td {
  width: var(--colsize);
  height: var(--colsize);

  font-size: 55%;
  font-weight: 600;
}

table th {
  border-right: 0.5rem solid #ba9771;
  background: #fff;

  color: hsla(0, 75%, 50%, 1);
}

table td {
  background: #000;
}

.col-button {
  width: var(--colsize);
  height: var(--colsize);

  padding: 0;

  border: 0;
  background: none;

  color: hsla(0, 0%, 60%, 1);
  font-size: 1em;
  font-weight: 600;

  cursor: url('./trey-corn.png'), auto;
}

.col-button:focus,
.col-button:hover {
  color: hsla(50, 100%, 90%, 1);
}

.col-button.active {
  color: hsla(50, 100%, 80%, 1);
}

.reset-button {
  cursor: url('./Trey-disappointed.png'), auto;
}
</style>
