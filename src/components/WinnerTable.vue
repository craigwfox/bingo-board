<template>
  <div class="winners-wrapper">
    <h2>Winners</h2>

    <form class="player-form">
      <div class="input">
        <label for="playername">Player name</label>
        <input type="text" id="playername" />
      </div>
      <button v-on:click="addPlayer">submit</button>
    </form>

    <table v-if="Object.keys(winnersarry).length > 0">
      <thead>
        <tr>
          <th>Player</th>
          <th>Rounds won</th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="person in winnersarry" v-bind:key="person.name">
          <td>{{ person.name }}</td>
          <td>{{ person.wins }}</td>
          <td>
            <button class="winner-edit" :data-playerid="person.id" v-on:click="addWin">
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
            <button class="winner-edit" :data-playerid="person.id" v-on:click="removeWin">
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
      v-if="Object.keys(winnersarry).length > 0"
      class="reset-button"
      v-on:click="clearPlayers"
    >
      Clear players
    </button>
  </div>
</template>

<script lang="ts">
export default {
  name: 'WinnerTable',
  data() {
    return {
      winnersarry: {},
    }
  },
  mounted() {
    if (window.localStorage.getItem('cachedwinners')) {
      this.winnersarry = JSON.parse(window.localStorage.getItem('cachedwinners'))
    }
  },
  methods: {
    addPlayer: function (e) {
      e.preventDefault()
      const playerinput = document.getElementById('playername'),
        playername = playerinput.value,
        playerid = playername.replace(/\s/g, '-').toLowerCase()

      if (playername.length > 0) {
        if (this.winnersarry[playerid]) {
          this.winnersarry[playerid].wins = this.winnersarry[playerid].wins + 1
        } else {
          Vue.set(this.winnersarry, playerid, {
            id: playerid,
            name: playername,
            wins: 1,
          })
        }

        playerinput.value = ''
        window.localStorage.setItem('cachedwinners', JSON.stringify(this.winnersarry))
      }
    },
    addWin: function (e) {
      e.preventDefault()
      const el = e.target,
        elId = el.getAttribute('data-playerid')

      this.winnersarry[elId].wins = this.winnersarry[elId].wins + 1
      window.localStorage.setItem('cachedwinners', JSON.stringify(this.winnersarry))
    },
    removeWin: function (e) {
      e.preventDefault()
      const el = e.target,
        elId = el.getAttribute('data-playerid')

      this.winnersarry[elId].wins = this.winnersarry[elId].wins - 1
      if (this.winnersarry[elId].wins === 0) {
        delete this.winnersarry[elId]
      }
      window.localStorage.setItem('cachedwinners', JSON.stringify(this.winnersarry))
    },
    clearPlayers: function (e) {
      e.preventDefault()

      if (window.confirm('You are about to reset the player table. To proceed press ok')) {
        this.winnersarry = {}
        window.localStorage.removeItem('cachedwinners')
      }
    },
  },
}
</script>

<style scoped>
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
}

.player-form button {
  margin-top: 1rem;

  padding: 0.5rem 1rem;

  border: 0;
  background: hsla(207, 67%, 43%, 1);

  color: #fff;
  font-size: 1.8rem;
  line-height: 1;
  cursor: pointer;
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

  cursor: pointer;
}

.winner-edit svg {
  width: 2.4em;
  height: 2.4em;

  font-size: 0.5rem;

  pointer-events: none;
}
</style>
