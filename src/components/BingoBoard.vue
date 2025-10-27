<template>
  <div>
    <table>
      <tbody>
        <tr>
          <th scope="row">B</th>
          <td v-for="col in rowrange(1, 15)" :key="col">
            <button class="col-button" :id="'b' + col" :data-row="'rowb'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">I</th>
          <td v-for="col in rowrange(16, 30)" :key="col">
            <button class="col-button" :id="'i' + col" :data-row="'rowi'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">N</th>
          <td v-for="col in rowrange(31, 45)" :key="col">
            <button class="col-button" :id="'n' + col" :data-row="'rown'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">G</th>
          <td v-for="col in rowrange(46, 60)" :key="col">
            <button class="col-button" :id="'g' + col" :data-row="'rowg'" v-on:click="highlightCol">
              {{ col }}
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">O</th>
          <td v-for="col in rowrange(61, 75)" :key="col">
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

<script lang="ts">
export default {
  name: 'BingoBoard',
  data() {
    return {
      index: 0,
      currentNumbers: {
        rowb: [],
        rowi: [],
        rown: [],
        rowg: [],
        rowo: [],
      },
    }
  },
  methods: {
    rowrange: (rangestart, rangestop) => {
      const rangearry = []
      for (let i = rangestart; i <= rangestop; i++) {
        rangearry.push(i)
      }
      return rangearry
    },
    highlightCol: function (e) {
      const el = e.target,
        currow = el.getAttribute('data-row'),
        elId = el.getAttribute('id')

      if (el.classList.contains('active')) {
        const arrypos = this.currentNumbers[currow].indexOf(elId)

        el.classList.remove('active')
        this.currentNumbers[currow].splice(arrypos, 1)
      } else {
        el.classList.add('active')
        this.currentNumbers[currow].push(elId)
      }
    },
    gameReset: function () {
      const activeButtons = document.querySelectorAll('table td button.active')

      this.currentNumbers.rowb = []
      this.currentNumbers.rowi = []
      this.currentNumbers.rown = []
      this.currentNumbers.rowg = []
      this.currentNumbers.rowo = []

      activeButtons.forEach((element) => {
        element.classList.remove('active')
      })
    },
  },
}
</script>

<style scoped>
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

  cursor: pointer;
}

.col-button:focus,
.col-button:hover {
  color: hsla(50, 100%, 90%, 1);
}

.col-button.active {
  color: hsla(50, 100%, 80%, 1);
}
</style>
