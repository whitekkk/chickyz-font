<template>
  <div>
    <pop-up :wait="wait" :checkFull="checkFull" :checkName="checkName" :letPlay="letPlay" :color="color" :myAvatar="myAvatar" :f="f" :c="c" :selectFace="selectFace" :selectColor="selectColor" :waitingTime="waitingTime"></pop-up>
    <game :wait="wait" :mousePosition="mousePosition" :avatars="avatars" :myAvatar="myAvatar" :halfHeight="halfHeight" :halfWidth="halfWidth" :target="target" :foods="foods" :hOFs="hOFs" :ranking="ranking" :mapSize="mapSize" :mapResize="mapResize" :countStep="countStep"></game>
    <div id="fb-root"></div>
  </div>
</template>

<script>
(function (d, s, id) {
  var js = d.getElementsByTagName(s)[0]
  var fjs = d.getElementsByTagName(s)[0]
  if (d.getElementById(id)) return
  js = d.createElement(s); js.id = id
  js.src = '//connect.facebook.net/th_TH/sdk.js#xfbml=1&version=v2.8'
  fjs.parentNode.insertBefore(js, fjs)
}(document, 'script', 'facebook-jssdk'))

var Vue = require('vue')
Vue.config.debug = false
Vue.config.silent = true

import Game from './components/Game'
import PopUp from './components/PopUp'
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyCPjSZnxBY9KLykYc18iW4yNVTbQyaBPsU',
  authDomain: 'chickyz-afcfe.firebaseapp.com',
  databaseURL: 'https://chickyz-afcfe.firebaseio.com',
  storageBucket: 'chickyz-afcfe.appspot.com',
  messagingSenderId: '763551736427'
}
firebase.initializeApp(config)

var Avatars = firebase.database().ref('avatars')
var myId = ''
window.onbeforeunload = function () {
  firebase.database().ref('avatars/' + myId).remove()
}

var Foods = firebase.database().ref('foods')
var HOFs = firebase.database().ref('hofs')

export default {
  created () {
    window.addEventListener('keyup', this.upKey)
    window.addEventListener('keydown', this.downKey)
  },
  mounted () {
    let vm = this
    var roomCheck = 0
    var key = 'value'
    Avatars.on(key, function (snapshot) {
      roomCheck = snapshot.numChildren()
      if (roomCheck >= 100) {
        vm.checkFull = true
      }
    })
    key = 'child_moved'
    let x = Math.floor(Math.random() * 2750) + 50
    let y = Math.floor(Math.random() * 2788) + 50
    let avatar = {
      x,
      y
    }
    let result = Avatars.push(avatar)
    myId = result.key
    firebase.database().ref('avatars/' + myId).remove()
    // *warning
    Avatars.on('child_added', function (snapshot) {
      var item = snapshot.val()
      item.id = snapshot.key
      vm.avatars.push(item)
    })
    Avatars.on('child_changed', function (snapshot) {
      var id = snapshot.key
      var avatar = vm.avatars.find(avatar => avatar.id === id)
      avatar.x = snapshot.val().x
      avatar.y = snapshot.val().y
      avatar.color = snapshot.val().color
      avatar.face = snapshot.val().face
      avatar.speed = snapshot.val().speed
      avatar.eat = snapshot.val().eat
      avatar.score = snapshot.val().score
      // change
      if (vm.myAvatar.id === id) {
        vm.myAvatar.x = snapshot.val().x
        vm.myAvatar.y = snapshot.val().y
        vm.myAvatar.color = snapshot.val().color
        vm.myAvatar.face = snapshot.val().face
        vm.myAvatar.speed = snapshot.val().speed
        vm.myAvatar.eat = snapshot.val().eat
        vm.myAvatar.score = snapshot.val().score
        if (vm.myAvatar.score === undefined || vm.myAvatar.score < 0) {
          vm.myAvatar.id = ''
          vm.wait = true
          vm.waitingTime = 3
          vm.checkName = true
        }
      }
    })
    Avatars.on('child_removed', function (snapshot) {
      var id = snapshot.key
      vm.avatars.splice(vm.avatars.findIndex(avatar => avatar.id === id), 1)
      // vm.checkName = true ***`แก้ซะ`
    })
    Foods.on('child_added', function (snapshot) {
      var item = snapshot.val()
      item.id = snapshot.key
      vm.foods.push(item)
    })
    Foods.on('child_changed', function (snapshot) {
      var id = snapshot.key
      var food = vm.foods.find(food => food.id === id)
      food.color = snapshot.val().color
      food.pic = snapshot.val().pic
      food.x = snapshot.val().x
      food.y = snapshot.val().y
      // change
    })
    Foods.on('child_removed', function (snapshot) {
      var id = snapshot.key
      vm.foods.splice(vm.foods.findIndex(food => food.id === id), 1)
    })

    HOFs.on('child_added', function (snapshot) {
      var item = snapshot.val()
      item.id = snapshot.key
      vm.hOFs.push(item)
    })
    HOFs.on('child_changed', function (snapshot) {
      var id = snapshot.key
      var hof = vm.hOFs.find(hof => hof.id === id)
      hof.name = snapshot.val().name
      hof.score = snapshot.val().score
    })
    HOFs.on('child_removed', function (snapshot) {
      var id = snapshot.key
      vm.hOFs.splice(vm.hOFs.findIndex(hof => hof.id === id), 1)
    })
    vm.hOFs.sort((parameterOne, parameterTwo) => parameterTwo.score - parameterOne.score)
  },
  data () {
    let winHeight = window.innerHeight
    let winWidth = window.innerWidth
    let c = Math.floor(Math.random() * 3) + 1
    let f = Math.floor(Math.random() * 3) + 1
    let color = ''
    if (c === 1) {
      color = '#F5FF5D'
    } else if (c === 2) {
      color = '#AEFBE9'
    } else {
      color = '#FC665A'
    }

    return {
      buttonZ: false,
      countStep: 100,
      mapSize: 0.03,
      ranking: [],
      target: '',
      checkFull: false,
      halfHeight: winHeight / 2,
      halfWidth: winWidth / 2,
      mouseX: 0,
      mouseY: 0,
      avatars: [],
      foods: [],
      hOFs: [],
      checkName: true,
      active: false,
      waitingTime: 3,
      wait: true,
      color,
      face: '',
      f,
      c,
      myAvatar: {
        name: '',
        x: 0,
        y: 0,
        face: '',
        color,
        speed: false,
        eat: false,
        score: 0
      }
    }
  },

  components: {
    Game,
    PopUp
  },

  // methods
  methods: {
    selectFace (num) {
      this.f = this.f + num
      if (this.f > 3) {
        this.f = 1
      } else if (this.f <= 0) {
        this.f = 3
      }
    },
    selectColor (num) {
      this.c = this.c + num
      if (this.c > 3) {
        this.c = 1
      } else if (this.c <= 0) {
        this.c = 3
      }
      if (this.c === 1) {
        this.color = '#F5FF5D'
      } else if (this.c === 2) {
        this.color = '#AEFBE9'
      } else {
        this.color = '#FC665A'
      }
    },
    gameStart () {
      let vm = this
      let count = 0
      setInterval(function () {
        vm.halfHeight = window.innerHeight / 2
        vm.halfWidth = window.innerWidth / 2
        count++
        if (count === 3) {
          vm.ranking = vm.avatars
          vm.ranking.sort((parameterOne, parameterTwo) => parameterTwo.score - parameterOne.score)
          vm.ranking = vm.ranking.slice(0, 5)
          vm.hOFs.sort((parameterOne, parameterTwo) => parameterTwo.score - parameterOne.score)
          count = 0
        }
      }, 300)
      // *** ranks
      firebase.database().ref('avatars/' + myId).remove()
      vm.addAvatar(vm.myAvatar)
      vm.move(false)
      vm.foodsGen()
    },
    letPlay () {
      let vm = this
      if (typeof window.orientation === 'undefined') {
        vm.checkName = false
        let i = 0
        let waiting = setInterval(function () {
          vm.waitingTime--
          if (i === 2) {
            vm.gameStart()
            vm.wait = false
            clearInterval(waiting)
          }
          i++
        }, 1000)
      } else {
        vm.wait = true
        vm.waitingTime = 3
        vm.checkName = true
        window.alert('Sorry, this game not support in mobile device.')
      }
    },
    upKey (e) {
      if (this.waitingTime === 0) {
        if (e.keyCode === 77) {
          this.mapResize()
        }
        if (e.keyCode === 90) {
          this.speed()
        }
        if (e.keyCode === 88) {
          this.shutup()
        }
      }
      if (e.keyCode === 13) {
        if (this.checkName !== false) {
          this.letPlay()
        }
      }
    },
    downKey (e) {
      if (this.waitingTime === 0) {
        if (e.keyCode === 88) {
          this.eat()
        }
      }
    },
    addAvatar (newAvatar) {
      let vm = this
      let color = vm.color
      let face = vm.f
      vm.myAvatar.x = Math.floor(Math.random() * 2750) + 50
      vm.myAvatar.y = Math.floor(Math.random() * 2788) + 50
      vm.myAvatar.face = face
      vm.myAvatar.color = color
      vm.myAvatar.speed = false
      vm.myAvatar.eat = false
      vm.myAvatar.score = 0
      vm.myAvatar.name = vm.myAvatar.name.substring(0, 7)
      let result = Avatars.push(newAvatar)
      this.myAvatar.id = result.key
      myId = this.myAvatar.id
      if (vm.myAvatar.color === '#F5FF5D') {
        vm.target = '#AEFBE9'
      } else if (vm.myAvatar.color === '#AEFBE9') {
        vm.target = '#FC665A'
      } else {
        vm.target = '#F5FF5D'
      }
    },
    move () {
      let vm = this
      let xCenter = vm.halfWidth
      let yCenter = vm.halfHeight
      var xOrigin = vm.myAvatar.x
      var yOrigin = vm.myAvatar.y
      var x1 = vm.mouseX
      var y1 = vm.mouseY
      var step = 1
      if (vm.myAvatar.speed === true) {
        step = 4
      }
      var dx = Math.abs(x1 - xCenter)
      var dy = Math.abs(y1 - yCenter)
      var checkX = (xCenter < x1) ? step : -step
      var checkY = (yCenter < y1) ? step : -step
      var err = dx - dy
      let i = 1
      var e2 = 0
      clearInterval(vm.active)
      vm.active = setInterval(function () {
        if (!vm.myAvatar.speed && vm.countStep < 100 + vm.myAvatar.score) {
          step = 1
          vm.countStep++
        }
        if (vm.myAvatar.speed && vm.countStep > 0) {
          step = 4
          vm.countStep--
        } else {
          step = 1
          firebase.database().ref('avatars/' + vm.myAvatar.id).update({
            speed: false
          })
        }

        if (i > 10 || (x1 !== vm.mouseX || y1 !== vm.mouseY) || vm.countStep === 0 || vm.buttonZ) {
          vm.buttonZ = false
          xCenter = vm.halfWidth
          yCenter = vm.halfHeight
          x1 = vm.mouseX
          y1 = vm.mouseY
          dx = Math.abs(x1 - xCenter)
          dy = Math.abs(y1 - yCenter)
          checkX = (xCenter < x1) ? step : -step
          checkY = (yCenter < y1) ? step : -step
          err = dx - dy
          i = 0
        }
        i++
        if (!(((xCenter + 25 > x1) && (xCenter - 25 < x1)) && ((yCenter - 25 < y1) && (yCenter + 25 > y1)))) {
          if (vm.active && vm.myAvatar.id !== '') {
            firebase.database().ref('avatars/' + vm.myAvatar.id).update({
              y: yOrigin,
              x: xOrigin
            })
          }
          e2 = 2 * err
          if (e2 > -dy) {
            err -= dy
            xOrigin += checkX
          }
          if (e2 < dx) {
            err += dx
            yOrigin += checkY
          }
          // check out of area
          if (xOrigin < 0 && x1 < xCenter) {
            xOrigin = 0
          } else if (xOrigin > 2898 && x1 > xCenter) {
            xOrigin = 2898
          }

          if (yOrigin < -20 && y1 < yCenter) {
            yOrigin = -20
          } else if (yOrigin > 2860 && y1 > yCenter) {
            yOrigin = 2860
          }

          if (vm.myAvatar.color === undefined) {
            yOrigin = -500
          }
          if (vm.myAvatar.score === undefined) {
            clearInterval(vm.active)
          }
        }
      }, 10)
    },
    checkEat () {
      var vm = this
      var index = 0
      var check = 0
      // *chekeat food
      var eatFood = 0
      eatFood = vm.foods.find(food => {
        index++
        check = ((food.x < vm.myAvatar.x + 50) && (food.x > vm.myAvatar.x - 50)) && ((food.y < vm.myAvatar.y + 50) && (food.y > vm.myAvatar.y - 50))
        return (check)
      })
      vm.foods.splice(index, 0)
      if (eatFood !== undefined) {
        firebase.database().ref('foods/' + eatFood.id).remove()
        if (eatFood.color !== '') {
          if (vm.myAvatar.score < 5) {
            vm.myAvatar.score = -2
          }
          vm.myAvatar.score = Math.ceil(vm.myAvatar.score / 2)
          vm.myAvatar.color = eatFood.color
        } else {
          vm.myAvatar.score += 2
        }

        if (vm.myAvatar.color === '#F5FF5D') {
          vm.target = '#AEFBE9'
        } else if (vm.myAvatar.color === '#AEFBE9') {
          vm.target = '#FC665A'
        } else {
          vm.target = '#F5FF5D'
        }
        if (vm.myAvatar.id !== '') {
          firebase.database().ref('avatars/' + vm.myAvatar.id).update({
            color: vm.myAvatar.color,
            score: vm.myAvatar.score
          })
        }
      }
      // *chekeat chick
      var eatChick = 0
      index = 0
      check = 0
      eatChick = vm.avatars.find(avatar => {
        index++
        check = ((((avatar.x < vm.myAvatar.x + 50) && (avatar.x > vm.myAvatar.x - 50)) && ((avatar.y < vm.myAvatar.y + 50) && (avatar.y > vm.myAvatar.y - 50))) && avatar.id !== vm.myAvatar.id)
        return (check)
      })
      if (eatChick !== undefined) {
        if (vm.myAvatar.color === '#F5FF5D') {
          if (eatChick.color === '#AEFBE9') {
            firebase.database().ref('avatars/' + eatChick.id).remove()
            vm.myAvatar.score += Math.ceil((eatChick.score / 2)) + 10
            if (vm.myAvatar.id !== '') {
              firebase.database().ref('avatars/' + vm.myAvatar.id).update({
                score: vm.myAvatar.score
              })
            }
          }
        } else if (vm.myAvatar.color === '#AEFBE9') {
          if (eatChick.color === '#FC665A') {
            firebase.database().ref('avatars/' + eatChick.id).remove()
            vm.myAvatar.score += Math.ceil((eatChick.score / 2)) + 10
            if (vm.myAvatar.id !== '') {
              firebase.database().ref('avatars/' + vm.myAvatar.id).update({
                score: vm.myAvatar.score
              })
            }
          }
        } else if (vm.myAvatar.color === '#FC665A') {
          if (eatChick.color === '#F5FF5D') {
            firebase.database().ref('avatars/' + eatChick.id).remove()
            vm.myAvatar.score += Math.ceil((eatChick.score / 2) + 10)
            if (vm.myAvatar.id !== '') {
              firebase.database().ref('avatars/' + vm.myAvatar.id).update({
                score: vm.myAvatar.score
              })
            }
          }
        }
        if (Object.keys(eatChick).length === 3 || eatChick.score === undefined) {
          firebase.database().ref('avatars/' + eatChick.id).remove()
          vm.myAvatar.score += 5
          firebase.database().ref('avatars/' + vm.myAvatar.id).update({
            score: vm.myAvatar.score
          })
        }
      }
      if (vm.myAvatar.score < 0) {
        clearInterval(vm.active)
        firebase.database().ref('avatars/' + myId).remove()
      }
    },
    mousePosition () {
      let vm = this
      if (this.waitingTime === 0) {
        let position = window.event
        vm.mouseX = position.clientX
        vm.mouseY = position.clientY
        let face = vm.myAvatar.color.toString()
        vm.actionChick(vm, face)
      }
    },
    actionChick (vm, face) {
      if (this.wait !== true) {
        if (vm.myAvatar.eat && vm.myAvatar.face !== vm.f + '-0') {
          firebase.database().ref('avatars/' + vm.myAvatar.id).update({
            face: vm.myAvatar.face + '-0'
          })
        }
      }
    },
    speed () {
      this.buttonZ = true
      this.move()
      if (!this.myAvatar.speed) {
        // clearInterval(this.active)
        firebase.database().ref('avatars/' + this.myAvatar.id).update({
          speed: true
        })
      } else {
        firebase.database().ref('avatars/' + this.myAvatar.id).update({
          speed: false
        })
      }
    },
    eat () {
      this.myAvatar.face = this.myAvatar.face.toString()
      if (this.myAvatar.face.search('-0') === -1 && !this.myAvatar.eat) {
        firebase.database().ref('avatars/' + this.myAvatar.id).update({
          eat: true,
          face: this.myAvatar.face + '-0'
        })
      }
      this.checkEat()
      this.checkHOF()
    },
    shutup () {
      firebase.database().ref('avatars/' + this.myAvatar.id).update({
        face: this.myAvatar.face.replace('-0', ''),
        eat: false
      })
    },
    foodsGen () {
      var newFood
      var vm = this
      var length = 0
      var genfood = 0
      var color = ''
      setInterval(function () {
        if (length < 40) {
          genfood = Math.floor(Math.random() * 30) + 1
          if (genfood > 4) {
            genfood = (genfood % 2) + 4
          }
          if (genfood === 1) {
            color = '#F5FF5D'
          } else if (genfood === 2) {
            color = '#AEFBE9'
          } else if (genfood === 3) {
            color = '#FC665A'
          } else {
            color = ''
          }
          newFood = {
            pic: genfood,
            color,
            x: Math.floor(Math.random() * 2800) + 50,
            y: Math.floor(Math.random() * 2778) + 50
          }
          vm.addFood(newFood)
        }
        length = vm.foods.length
      }, 10000)
    },
    addFood (newFood) {
      Foods.push(newFood)
    },
    mapResize () {
      let vm = this
      if (vm.mapSize === 0.03) {
        vm.mapSize = 0.08
      } else {
        vm.mapSize = 0.03
      }
    },
    checkHOF () {
      var vm = this
      if (vm.hOFs.length < 3) {
        firebase.database().ref('hofs/' + this.myAvatar.id).update({
          name: vm.myAvatar.name,
          score: vm.myAvatar.score
        })
      } else if (vm.myAvatar.score > vm.hOFs[2].score) {
        if (this.myAvatar.id !== vm.hOFs[2].id && this.myAvatar.id !== vm.hOFs[1].id && this.myAvatar.id !== vm.hOFs[0].id) {
          firebase.database().ref('hofs/' + vm.hOFs[2].id).remove()
        }
        firebase.database().ref('hofs/' + this.myAvatar.id).update({
          name: vm.myAvatar.name,
          score: vm.myAvatar.score
        })
      }
    }
  }
}
</script>
<style src="../static/css/game.css"> </style>
