<template>
  <MDBTabs v-model="activeTabId2">
    <!-- Tabs navs -->
    <MDBTabNav fill tabsClasses="mb-3">
      <MDBTabItem tabId="ex2-1" href="ex2-1">Timer</MDBTabItem>
      <MDBTabItem tabId="ex2-2" href="ex2-2">Short Break</MDBTabItem>
      <MDBTabItem tabId="ex2-3" href="ex2-3">Long Break</MDBTabItem>
    </MDBTabNav>
    <!-- Tabs navs -->
    <!-- Tabs content -->
    <MDBTabContent>
      <MDBTabPane tabId="ex2-1">
        <h1 class="display-1">{{ displayTimer }}</h1>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="primary" @click="start(timers[0]['name'])" :disabled="isDisable">
            <MDBIcon class="text-start" icon="play-circle" iconStyle="far" />
              Start
            </MDBBtn>
        </MDBBtnGroup>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="danger" @click="stop">
            <MDBIcon icon="stop-circle" iconStyle="far" />
            Stop
          </MDBBtn>
        </MDBBtnGroup>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="light" @click="reset(timers[0]['name'])">
            <MDBIcon icon="redo" iconStyle="fas" />
            Reset
          </MDBBtn>
        </MDBBtnGroup>
      </MDBTabPane>

      <MDBTabPane tabId="ex2-2">
        <h1 class="display-1">{{ displayShortBreak }}</h1>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="primary" @click="start(timers[1]['name'])" :disabled="isDisable">
            <MDBIcon class="text-start" icon="play-circle" iconStyle="far" />
              Start
            </MDBBtn>
        </MDBBtnGroup>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="danger" @click="stop">
            <MDBIcon icon="stop-circle" iconStyle="far" />
            Stop
          </MDBBtn>
        </MDBBtnGroup>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="light" @click="reset(timers[1]['name'])">
            <MDBIcon icon="redo" iconStyle="fas" />
            Reset
          </MDBBtn>
        </MDBBtnGroup>
      </MDBTabPane>

      <MDBTabPane tabId="ex2-3">
        <h1 class="display-1">{{ displayLongBreak }}</h1>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="primary" @click="start(timers[2]['name'])" :disabled="isDisable">
            <MDBIcon class="text-start" icon="play-circle" iconStyle="far" />
              Start
            </MDBBtn>
        </MDBBtnGroup>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="danger" @click="stop">
            <MDBIcon icon="stop-circle" iconStyle="far" />
            Stop
          </MDBBtn>
        </MDBBtnGroup>
        <MDBBtnGroup class="mt-3 mx-1">
          <MDBBtn color="light" @click="reset(timers[2]['name'])">
            <MDBIcon icon="redo" iconStyle="fas" />
            Reset
          </MDBBtn>
        </MDBBtnGroup>
      </MDBTabPane>

    </MDBTabContent>
    <!-- Tabs content -->
  </MDBTabs>

  <SettingsDialog :timers="timers" :save="save" />

</template>

<script>
  import {
    MDBTabs,
    MDBTabNav,
    MDBTabContent,
    MDBTabItem,
    MDBTabPane,
    MDBBtnGroup,
    MDBBtn,
    MDBIcon,
  } from 'mdb-vue-ui-kit';
  import { ref } from 'vue';
  import SettingsDialog from './SettingsDialog.vue'

  export default {
    components: {
      MDBTabs,
      MDBTabNav,
      MDBTabContent,
      MDBTabItem,
      MDBTabPane,
      MDBBtnGroup,
      MDBBtn,
      MDBIcon,
      SettingsDialog
    },
    setup() {
      const activeTabId2 = ref('ex2-1')

      return {
        activeTabId2,
      }
    },
    data(){
        return {
            resetTimer: [
              25 * 60,
              5 * 60,
              10 * 60
            ],
            timers:[
              {
                name: 'pomodoro',
                seconds: 25 * 60
              },
              {
                name: 'shortbreak',
                seconds: 5 * 60
              },
              {
                name: 'longbreak',
                seconds: 10 * 60
              }
            ],
            intervalInstance: null,
            isDisable: false,
        }
    },
    methods: {
      start(timer){
          const time = this.timers.find(t => t.name == timer).seconds
          this.isDisable = true
          this.intervalInstance = setInterval(() => {
            this.timers.find(t => t.name == timer).seconds -= 1
            if(this.timers.find(t => t.name == timer).seconds == 0){
              this.stop()
              this.timers.find(t => t.name == timer).seconds = time
            }
          } ,1000)
      },
      stop() {
        this.isDisable = false
        clearInterval(this.intervalInstance)
      },
      reset(timerType) {
        this.isDisable = false
        clearInterval(this.intervalInstance)
        if (timerType == 'pomodoro'){
          this.timers[0]['seconds'] = this.resetTimer[0]
        } else if (timerType == 'shortbreak') {
          this.timers[1]['seconds'] = this.resetTimer[1]
        } else {
          this.timers[2]['seconds'] = this.resetTimer[2]
        }
      },
      displayMinutes(time) {
        const minutes = Math.floor(time/60)
        if (minutes < 10) {
          return `0${minutes}`
        }
        return `${minutes}`
      },
      displaySeconds(time) {
        const seconds = time % 60
        if (seconds < 10) {
          return `0${seconds}`
        }
        return `${seconds}`
      },
      save(updatedTimers){
        this.resetTimer = updatedTimers.map(timer => timer.seconds * 60)
        this.timers = updatedTimers.map((timer, i) => {
          return {...timer, seconds: updatedTimers[i]['seconds'] * 60}
        })
      }
    },
    computed: {
      displayTimer(){
          return `${this.displayMinutes(this.timers[0]['seconds'])}:${this.displaySeconds(this.timers[0]['seconds'])}`
      },
      displayShortBreak(){
          return `${this.displayMinutes(this.timers[1]['seconds'])}:${this.displaySeconds(this.timers[1]['seconds'])}`
      },
      displayLongBreak(){
          return `${this.displayMinutes(this.timers[2]['seconds'])}:${this.displaySeconds(this.timers[2]['seconds'])}`
      }
    },
  }
</script>

<style scoped>
  h1 {
    margin-top: 30px;
    font-size: calc(2.625rem + 6.5vw);
  }
</style>