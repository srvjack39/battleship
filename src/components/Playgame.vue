<template>
  <div class="hello">
    <h1 class="title has-text-light">Battleship</h1>
    <h2 class="subtitle has-text-primary">Let fun with me</h2>
    <h2 class="subtitle has-text-light">score = {{score}} - 0</h2>
    <div class="columns is-mobile is-centered">
      <div class="column is-6">

        <h1 class="title has-text-light" v-if="score==9">You win</h1>
        <h2 class="subtitle has-text-light">me</h2>
        <table>
          <tr v-for="(y, indexY) in getOwn" :key="y['.key']">
            <td v-for="(x, indexX) in y" :key="x['.key']" :class="">
              <img v-if="x.shipstatus" src="../assets/ship.png" class="img">
            </td>
          </tr>
        </table>
      </div>
      <div class="column is-6">
        <h2 class="subtitle has-text-light">enemy</h2>
        <table>
          <tr v-for="(y, indexY) in getEnemy" :key="y['.key']">
            <td v-for="(x, indexX) in y" :key="x['.key']" @click="setbomb(indexX,indexY,x)" :class="setClass(x)">
              <img v-if="x.shipstatus" src="../assets/ship.png" class="img">
            </td>
          </tr>
        </table>
      </div>
    </div>
    <br><br>
    <div class="columns is-mobile is-centered menu">
      <div class="field is-grouped">
        <div class="control">
          <button class="button is-primary is-large" @click="">Finish</button>
        </div>
      </div>
    </div>
  <br>

  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  name: 'Placeship',
  data () {
    return {
      x: 0,
      y: 0,
      score: 0,
      boardOnplay: '0011',
      ShipEnemy: '',
      hidemenu: [true, true, true]
    }
  },
  methods: {
    ...mapActions([
      'setbombFirebase'
    ]),
    setbomb (x, y, obj) {
      if (obj.shipstatus && !obj.bombstatus) {
        this.score++
      }
      var xy = {
        x: x,
        y: y
      }
      this.setbombFirebase(xy)
    },
    showconsole (x, y) {
      console.log(x + ',' + y)
    },
    select (x, y) {
      this.x = x
      this.y = y
    },
    setClass (obj) {
      if (obj.shipstatus && obj.bombstatus) {
        return 'boom'
      } else if (obj.bombstatus) {
        return 'empty'
      } else {
        return ''
      }
    }
  },
  firebase: {
  },
  created () {
    // this.enemysea = this.getShipEnemy()
    /* for (var i = 0; i < 10; i++) {
      this.position.push([])
      this.position[i].push(new Array(10))
      for (var j = 0; j < 10; j++) {
        this.position[i][j] = {
          shipstatus: false, bombstatus: false
        }
        console.log(this.position[i][j].shipstatus)
      }
    } */
  },
  computed: {
    ...mapGetters([
      'getEnemy',
      'getOwn'
    ])
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  td{
    border:2px solid rgba(0, 204, 255, 0.1);
    width:50px;
    height:50px;
  }
  table{
    width:auto;
    height:auto;
  }
  .img {
    width:40px;
    height:40px;
  }
  .ship{
    background-color: rgb(194, 70, 174);
    margin: 10px;
  }
  .boom {
    background-color: rgba(203, 74, 10, 0.74);
  }
  .empty {
    background-color: rgba(70, 142, 194,0.3);
  }
</style>
