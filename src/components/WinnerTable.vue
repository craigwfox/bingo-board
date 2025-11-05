<template>
  <div class="winners-wrapper">
    <h2>Winners</h2>

    <form class="player-form" @submit.prevent="addPlayer">
      <div class="input">
        <label for="playerName">Player name</label>
        <input type="text" id="playerName" v-model="playerName" />
      </div>
      <button @click.prevent="addPlayer" type="button">submit</button>
    </form>

    <table v-if="winners && Object.keys(winners).length > 0">
      <thead>
        <tr>
          <th>Player</th>
          <th>Rounds won</th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="person in winners" v-bind:key="person.name">
          <td>{{ person.name }}</td>
          <td>{{ person.wins }}</td>
          <td>
            <button
              class="winner-edit winner-edit--add"
              type="button"
              @click.prevent="addWin(person.id)"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="#000000"
                stroke-width="3"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <line x1="12" y1="5" x2="12" y2="19"></line>
                <line x1="5" y1="12" x2="19" y2="12"></line>
              </svg>
            </button>
          </td>
          <td>
            <button
              class="winner-edit winner-edit--remove"
              type="button"
              @click.prevent="removeWin(person.id)"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="#000000"
                stroke-width="3"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <line x1="5" y1="12" x2="19" y2="12"></line>
              </svg>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <button
      v-if="Object.keys(winners).length > 0"
      class="reset-button"
      type="button"
      @click.prevent="clearPlayers"
    >
      Clear players
    </button>
  </div>
</template>

<script lang="ts" setup>
// ====---------------====
// 🚀 Imports
// ====---------------====
import { onMounted, ref } from 'vue'

// ====---------------====
// 💾 Types & Interfaces
// ====---------------====
interface Player {
  id: string
  name: string
  wins: number
}

interface Winners {
  [key: string]: Player
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
const playerName = ref('')
const winners = ref<Winners>({})

// ====---------------====
// 🛠 Methods
// ====---------------====
function addPlayer(e) {
  e.preventDefault()
  const playerIid = playerName.value.replace(/\s/g, '-').toLowerCase()

  if (winners.value?.[playerIid]) {
    winners.value[playerIid].wins += 1
  } else {
    winners.value[playerIid] = {
      id: playerIid,
      name: playerName.value,
      wins: 1,
    }

    playerName.value = ''
    cacheWinners()
  }
}

function addWin(id: string) {
  if (winners.value[id]) winners.value[id].wins += 1
  cacheWinners()
}

function removeWin(id: string) {
  if (winners.value[id]) {
    winners.value[id].wins -= 1
    if (winners.value[id].wins < 1) delete winners.value[id]
  }
  cacheWinners()
}

function clearPlayers() {
  winners.value = {}
  window.localStorage.removeItem('cachedwinners')
}

function cacheWinners() {
  window.localStorage.setItem('cachedwinners', JSON.stringify(winners.value))
}

// ====---------------====
// 🌄 Lifecycle
// ====---------------====
onMounted(() => {
  if (window.localStorage.getItem('cachedwinners'))
    winners.value = JSON.parse(window.localStorage.getItem('cachedwinners') ?? '')
})
</script>

<style lang="scss" scoped>
.winners-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.player-form {
  display: flex;
  flex-direction: column;
  align-items: flex-start;

  margin-bottom: 3rem;
}

.input {
  display: flex;
  flex-direction: column;
  align-items: flex-start;

  text-align: left;
}

.input label {
  line-height: 1;
}

.input input {
  margin-top: 0.5rem;
  padding: 0.5rem 0.5rem;

  font-size: 1.8rem;
  line-height: 1;

  cursor: url('./yodel-trey-head.png'), auto;
}

.player-form button {
  margin-top: 1rem;

  padding: 0.5rem 1rem;

  border: 0;
  background: hsla(207, 67%, 43%, 1);

  color: #fff;
  font-size: 1.8rem;
  line-height: 1;
  cursor: url('./trey-beard.png'), auto;
}

.player-form button:hover,
.player-form button:focus {
  background: hsla(209, 68%, 24%, 1);
}

h2 {
  margin-top: 6rem;
}

table {
  border-collapse: collapse;
}

th,
td {
  padding: 0.5rem 1rem;
}

thead th {
  background: hsla(209, 68%, 24%, 1);
  border: 0.1rem solid hsla(209, 68%, 24%, 1);
  border-right: 0.1rem solid hsla(209, 68%, 34%, 1);

  color: #fff;
}

thead th:empty {
  border-right: 0.1rem solid hsla(209, 68%, 24%, 1);
}

thead th:last-child {
  border-right: 0.1rem solid hsla(209, 68%, 24%, 1);
}

tbody th,
tbody td {
  border: 0.1rem solid #777;
}

tbody td:nth-last-child(-n + 2) {
  padding: 0;
}

.winner-edit {
  width: 4rem;
  height: 3rem;

  display: flex;
  align-items: center;
  justify-content: center;

  padding: 0.5rem 1rem;

  background: none;
  border: 0;

  cursor: url('./trey-beard.png'), auto;

  &--add {
    cursor: url('./tregg-icon.png'), auto;
  }

  &--remove {
    cursor: url('./treyofdisapproval.png'), auto;
  }
}

.winner-edit svg {
  width: 2.4em;
  height: 2.4em;

  font-size: 0.5rem;

  pointer-events: none;
}

.reset-button {
  cursor: url('./treyofdisapproval-bw.png'), auto;
}
</style>
