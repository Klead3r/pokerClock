<template>
  <v-container
    id="tournamentWrapper"
    class="tournamentWrapper"
    fluid
  >
    <v-btn
      color="pink"
      dark
      class="menuBtn"
      @click.stop="menu = !menu"
    >
      Menu
    </v-btn>
    <v-layout
      wrap
      row
      justify-center
      align-center
      fill-height
      class="text-xs-center"
    >
      <v-navigation-drawer
        v-model="menu"
        absolute
        temporary
      >
        <v-layout
          wrap
          row
          justify-center
          class="text-xs-center"
        >
          <v-flex
            xs12
          >
            <v-btn
              v-if="!clockRunning"
              block
              dark
              color="green lighten-2"
              @click="startClock"
            >
              Start Clock
            </v-btn>
            <v-btn
              v-if="clockRunning"
              block
              @click="stopClock"
            >
              Stop Clock
            </v-btn>
            <br>
            <v-btn
              block
              @click="blindRaiseAlert"
            >
              Test Horn
            </v-btn>
            <br />
            <span
              class="menuTitle"
            >
              Time: {{ extraZero }}{{ clockTime.minutes }}:{{ clockTime.secondsTens }}{{ clockTime.seconds }}
            </span>
            <br>
            <v-btn
              color="blue"
              dark
              @click="decMinute"
            >
              -1min
            </v-btn>
            <v-btn
              @click="incMinute"
            >
              +1min
            </v-btn>
            <br>
            <v-btn
              color="blue"
              dark
              @click="decTenSec"
            >
              -10sec
            </v-btn>
            <v-btn
              @click="incTenSec"
            >
              +10sec
            </v-btn>
            <br />
            <span
              class="menuTitle"
            >
              Level: {{ blinds.level + 1 }}
            </span>
            <br>
            <v-btn
              color="blue"
              dark
              @click="blinds.level++"
            >
              +
            </v-btn>
            <v-btn
              @click="blinds.level--"
            >
              -
            </v-btn>
            <br>
            <span
              class="menuTitle"
            >
              Players: {{ tournamentInfo.players }}
            </span>
            <br>
            <v-btn
              color="blue"
              dark
              @click="tournamentInfo.players++"
            >
              +
            </v-btn>
            <v-btn
              @click="tournamentInfo.players--"
            >
              -
            </v-btn>
            <br>
            <span
              class="menuTitle"
            >
              Re-Entries: {{ tournamentInfo.reEntries }}
            </span>
            <br>
            <v-btn
              color="blue"
              dark
              @click="tournamentInfo.reEntries++"
            >
              +
            </v-btn>
            <v-btn
              @click="tournamentInfo.reEntries--"
            >
              -
            </v-btn>
             <v-layout
              row
              wrap
            >
              <v-flex
                xs12
              >
                <span
                  class="menuTitle"
                >
                  Background color
                </span>
                <br>
                <v-btn
                  v-for="(btn, index) in colors"
                  :key="index"
                  :color="btn.value"
                  @click="switchBackgroundColor(btn.value)"
                />
              </v-flex>
              <v-flex
                xs12
              >
                <span
                  class="menuTitle"
                >
                  Text color
                </span>
                <br>
                <v-btn
                  v-for="(btn, index) in colors"
                  :key="index"
                  :color="btn.value"
                  @click="switchTextColor(btn.value)"
                />
              </v-flex>
              <v-flex
                xs12
              >
                <span
                  class="menuTitle"
                >
                  Highlight color
                </span>
                <br>
                <v-btn
                  v-for="(btn, index) in colors"
                  :key="index"
                  :color="btn.value"
                  @click="highlighColor = btn.value"
                />
              </v-flex>
            </v-layout>
            <br>
            <br>
            <br>
            <v-dialog
              v-model="resetDialog"
              width="300"
            >
              <template v-slot:activator="{ on }">
                <v-btn
                  color="red lighten-2"
                  dark
                  block
                  v-on="on"
                >
                  Reset timer
                </v-btn>
              </template>
              <v-card>
                <v-card-title
                  class="headline grey lighten-2 text-xs-center"
                  block
                >
                  Reset timer?
                </v-card-title>
                <v-layout
                  row
                  wrap
                >
                  <v-flex xs6>
                    <v-btn
    
                      block
                      @click="resetTimer(); resetDialog = false"
                    >
                      Yes
                    </v-btn>
                  </v-flex>
                  <v-flex xs6>
                    <v-btn
    
                      block
                      @click="resetDialog = false"
                    >
                      No
                    </v-btn>
                  </v-flex>
                </v-layout>
              </v-card>
            </v-dialog>
          </v-flex>
        </v-layout>
      </v-navigation-drawer>
      <v-flex
        xs3
      >
        <v-flex
          xs12
          class="tournamentInfo"
        >
          <p>Players <br>
            <span
              class="value"
            >
              {{ tournamentInfo.players }}
            </span>
          </p>
          <p>Re-Entries <br>
            <span
              class="value"
            >
              {{ tournamentInfo.reEntries }}
            </span>
          </p>
          <p>Starting Stack <br>
            <span
              class="value"
            >
              {{ tournamentInfo.stack }}
            </span>  
          </p>
          <p>Total Stack <br>
            <span
              class="value"
            >
              {{ totalStack }}
            </span>
          </p>
          <p>Buyin <br>
            <span
              class="value"
            >
              {{ tournamentInfo.buyin }}€
            </span>
          </p>
          <p>Prizepool <br>
            <span
              class="value"
            >
              {{ prizePool }}€
            </span>
          </p>
          <p>PRIZES
            <br>
            <span
              v-for="(prize, index) in prizes"
              :key="index"
              class="value"
            >
              {{ index + 1 }}. {{ prize }}€
            </span>
          </p>
        </v-flex>
      </v-flex>
      <v-flex
        xs6
      >
        <v-flex
          xs12
        >
          <span
            class="clockTime"
          >
            {{ extraZero }}{{ clockTime.minutes }}:{{ clockTime.secondsTens }}{{ clockTime.seconds }}
          </span>
        </v-flex>
        <v-flex
          xs12
        >
          <span
            class="currentBlinds"
          >
            {{ blinds.list[blinds.level] }}
          </span>
        </v-flex>
      </v-flex>
      <v-flex
        xs3
        class="blindList"
      >
        <span
          v-for="(blindLevel, index) in blinds.list"
          :key="index"
          :style="index === blinds.level ? { color: highlighColor} : ''"
        >
          {{ blindLevel }}
        </span>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      menu: false,
      resetDialog: false,
      clockRunning: false,
      timeOut: '',
      blinds: {
        list: [
          '10/20',
          '15/30',
          '20/40',
          '25/50',
          '30/60',
          '40/80',
          '50/100',
          '60/120',
          '75/150',
          '100/200',
          '125/250',
          '150/300',
          '200/400',
          '300/600',
          '400/800',
          '500/1000'
        ],
        duration: 15,
        level: 0
      },
      clockTime: {
        minutes: 0,
        secondsTens: 0,
        seconds: 0
      },
      tournamentInfo: {
        players: 0,
        reEntries: 0,
        stack: 1000,
        buyin: 10
      },
      colors: [
        {
          color: 'black',
          value: '#000'
        },
        {
          color: 'white',
          value: '#fff'
        },
        {
          color: 'red',
          value: 'red'
        },
        {
          color: 'blue',
          value: 'blue'
        },
        {
          color: 'green',
          value: 'green'
        },
        {
          color: 'yellow',
          value: 'yellow'
        }
      ],
      highlighColor: 'red'
    }
  },
  computed: {
    extraZero () {
      if (this.clockTime.minutes < 10) {
        return 0
      } else {
        return ''
      }
    },
    totalStack () {
      let totalStack = (this.tournamentInfo.players + this.tournamentInfo.reEntries) * this.tournamentInfo.stack
      return totalStack
    },
    prizePool () {
      let pricePool = (this.tournamentInfo.players + this.tournamentInfo.reEntries) * this.tournamentInfo.buyin
      return pricePool
    },
    prizes () {
      let prizeArray = []
      if (this.tournamentInfo.players < 6) {
        prizeArray.push(this.prizePool)
      } else if (this.tournamentInfo.players < 8) {
        prizeArray.push(Math.round(this.prizePool / 3 * 2 / 10) * 10)
        prizeArray.push(Math.round(this.prizePool / 3 / 10) * 10)
      } else {
        prizeArray.push(Math.round(this.prizePool * 0.5 / 5) * 5)
        prizeArray.push(Math.round(this.prizePool * 0.32 / 5) * 5)
        prizeArray.push(Math.round(this.prizePool * 0.18 / 5) * 5)
      }
      return prizeArray
    }
  },
  mounted () {
    this.clockTime.minutes = this.blinds.duration
  },
  methods: {
    startClock () {
      this.clockRunning = true
      var that = this
      this.timeOut = setTimeout(function () {
        if (that.clockTime.seconds === 0) {
          that.clockTime.seconds = 9
          if (that.clockTime.secondsTens === 0) {
            if (that.clockTime.minutes === 0) {
              that.clockTime.minutes = that.blinds.duration
              that.clockTime.secondsTens = 0
              that.clockTime.seconds = 0
              that.blinds.level++
              that.blindRaiseAlert()
            } else {
              that.clockTime.minutes--
              that.clockTime.secondsTens = 5
            }
          } else {
            that.clockTime.secondsTens--
          }
        } else {
          that.clockTime.seconds--
        }
        that.startClock()
      }, 1000)
    },
    stopClock () {
      clearTimeout(this.timeOut)
      this.clockRunning = false
    },
    resetTimer () {
      clearTimeout(this.timeOut)
      this.clockRunning = false
      this.clockTime = {
        minutes: 0,
        secondsTens: 0,
        seconds: 0
      }
      this.blinds.level = 0
    },
    incMinute () {
      this.clockTime.minutes++
    },
    decMinute () {
      if (this.clockTime.minutes > 0) this.clockTime.minutes--
    },
    incTenSec () {
      this.clockTime.secondsTens++
      if (this.clockTime.secondsTens > 5) {
        this.clockTime.secondsTens = 0
        this.clockTime.minutes++
      }
    },
    decTenSec () {
      if (this.clockTime.secondsTens > 0) {
        this.clockTime.secondsTens--
      }
    },
    blindRaiseAlert () {
      let audio = new Audio('/sound/blinds-horn.wav')
      audio.play()
    },
    switchBackgroundColor (color) {
      document.getElementById('tournamentWrapper').style.backgroundColor = color
    },
    switchTextColor (color) {
      document.getElementById('tournamentWrapper').style.color = color
    }
  }
}
</script>

<style lang="scss" scoped>
  .tournamentWrapper {
    height: 100%;
    .menuBtn {
      position: absolute;
      top: 0;
      left: 0;
    }
    .menuTitle {
      font-size: 16px;
      font-weight: 700;
      line-height: 1;
      color: #000;
    }
    .tournamentInfo {
      p {
        font-size: 20px;
        font-weight: 700;
        line-height: 1;
        .value {
          padding-top: 10px;
          display: block;
          font-size: 35px;
          font-weight: 700;
          line-height: 1;
        }
      }
    }
    .clockTime {
      font-size: 250px;
      font-weight: 700;
      line-height: 1;
    }
    .currentBlinds {
      font-size: 125px;
      font-weight: 700;
      line-height: 1;
    }
    .blindList {
      span {
        display: block;
        font-size: 35px;
        font-weight: 700;
        line-height: 1;
        padding-bottom: 5px
      }
    }
  }
</style>
