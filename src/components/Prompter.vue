<template>
  <div class="prompter">
    {{ elapsedTime.asSeconds() }}
    <button @click="startTime = moment()">start</button>
    <h1>short break, brb</h1>
    <div id="run" v-if="currentNotes">
      <div class="block" v-for="block in currentNotes.blocks" v-bind:key="block.text">
        <div class="time">{{ block.originalTime }}</div>
        <div class="block-text" v-html="block.text"></div>
        <hr>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
export default {
  name: 'Prompter',
  props: {},
  data () {
    return {
      rawNotes: [
        {
          settings: {},
          blocks: [
            {
              text: "skip fury",
              icon: "fury",
              time: "0:00:04"
            },
            {
              text: "get VS",
              icon: "vs",
              time: "0:00:20"
            },
            {
              text: "greenpath",
              icon: "vs",
              time: "0:00:30"
            },
            {
              text: "hornet",
              icon: "vs",
              time: "0:00:40"
            },
            {
              text: "claw",
              icon: "vs",
              time: "0:00:50"
            }
          ]
        }
      ],
      currentNotes: null,
      startTime: null,
      elapsedTime: moment.duration(),
      extraTime: moment.duration(),
      moment,
    }
  },
  computed: {
    notes () {
      return this.rawNotes.map(category => {
        return {
          settings: category.settings,
          blocks: category.blocks.map(block => {
            return {
              text: block.text,
              icon: block.icon,
              originalTime: block.time,  // TODO real formatting
              time: moment.duration(block.time)
            }
          })
        }
      })
    },
  },
  mounted () {
    this.currentNotes = this.notes[0]
    window.requestAnimationFrame(this.elapseTime)
  },
  methods: {
    elapseTime () {
      if (this.startTime) {
        const result = moment().subtract(this.startTime)
        this.elapsedTime = moment.duration(result)
      }
      window.requestAnimationFrame(this.elapseTime)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.block {
  height: 400px;
}
</style>
