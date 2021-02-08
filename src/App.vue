<template>
  <div id="app">
    <h4>🕑</h4>
    <h2 class="mono">{{ timeStr }}</h2>
    <h4>⏳</h4>
    <h2><span class="mono">{{ new Date(savedTime + clockTime).toISOString().substr(11, 12) }}</span> @<span class="mono">{{ wage.toLocaleString('en-US', { style: 'currency', currency: 'USD' }) }}</span>/hour</h2>
    <h4>💰</h4>
    <div class="balance mono">${{ getBalance().toFixed(5).toLocaleString() }}</div>
    <div class="button" @click="toggleClock()">Clock {{ clockedIn ? 'out' : 'in' }}</div>
    <div style="text-align: center;">
      <p>Refresh the page to reset.</p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      wage: 50,
      savedTime: 0,
      clockTime: 0,
      timeStr: '',
      clockedIn: false,
      clockInTime: false,
    }
  },
  methods: {
    toggleClock() {
      this.clockedIn = !this.clockedIn
      if (this.clockedIn) {
        this.clockInTime = new Date().getTime()
      }
      else {
        this.clockInTime = false
        this.savedTime += this.clockTime
        this.clockTime = 0
      }
    },
    getBalance() {
      return ((this.clockTime + this.savedTime) / (60 * 60 * 1000)) * this.wage
    },
    tick() {
      const now = new Date().getTime()
      this.timeStr = new Date().toISOString()
      if (this.clockedIn) {
        this.clockTime = now - this.clockInTime
      }
    }
  },
  mounted() {
    setInterval(() => {
      this.tick()
    }, 9)
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: auto;
  max-width: 600px;
  text-align: left;
  padding: 1rem;
}
.balance {
  font-size: 5rem;
  font-weight: bold;
}
.mono {
  font-family: 'JetBrains Mono', monospace;
}
.button {
  background-color: #000;
  color: #fff;
  border-radius: 4px;
  cursor: pointer;
  padding: 1.5rem 4rem;
  margin-top: 2rem;
  font-size: 2.5rem;
  text-align: center;
}
h4 {
  font-weight: bold;
}
</style>
