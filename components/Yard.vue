<template>
  <div class="yard">
    <div v-show="message" class="message">{{ message }}</div>
    <Playground :key="renderKey" :digits="tagArray"/>
    <button class="new-game__btn" @click="startNewGame()">New game</button>
  </div>
</template>

<script>
import Playground from './Playground'
  export default {
    components: {
      Playground
    },
    data: () => ({
      tagArray: null,
      renderKey: true,
      squareSideLength: 4, 
      emptyCellIndex: null,
      message: null
    }),
    mounted() {
      this.startNewGame()
      document.addEventListener('keyup', (e) => {
        this.handleKeyPress(e)
      })
    },
    methods: {
      startNewGame() {
        this.fillArrayRandomly()
        this.setEmptyCellIndex()
      },
      isSortedArray() {
        const arr = this.tagArray.slice(0, this.tagArray.length-1)
        return arr.every((val, i, arr) => !i || (val >= arr[i - 1]))
      },
      setEmptyCellIndex() {
        this.emptyCellIndex = this.tagArray.findIndex((value) => value === 0)
      },
      fillArrayRandomly() {
        this.tagArray = Array.from({length: this.squareSideLength ** 2}, (v, k) => k)
        this.tagArray = this.tagArray.sort(() => Math.random() - 0.5)
      },
      moveCell(newEmptyCellIndex, oldEmptyCellIndex) {
        const buffer = this.tagArray[newEmptyCellIndex]
        this.tagArray[newEmptyCellIndex] = 0
        this.tagArray[oldEmptyCellIndex] = buffer
      },
      isAbleToBeMoved(direction) {
        switch (direction) {
          case 'right':
            if (this.emptyCellIndex === 0 || this.emptyCellIndex % 4 === 0) {
              return false
            }
            else {
              return true 
            }
            break;
          case 'left':
            if (this.emptyCellIndex === this.tagArray.length - 1 || (this.emptyCellIndex + 1) % 4 === 0) {
              return false
            }
            else {
              return true 
            }
            break;
          case 'up':
            if (this.emptyCellIndex >= (this.tagArray.length - this.squareSideLength)) {
              return false
            }
            else {
              return true 
            }
            break;
          case 'down':
            if (this.emptyCellIndex < this.squareSideLength) {
              return false
            }
            else {
              return true 
            }
            break;
        }
      },
      handleKeyPress(e) {
        let newEmptyCellIndex = null
        let oldEmptyCellIndex = null

        if (e.keyCode == '38') {
          // Up arrow
          if (!this.isAbleToBeMoved('up')) {
            return
          }
          newEmptyCellIndex = this.emptyCellIndex + this.squareSideLength
          oldEmptyCellIndex = this.emptyCellIndex
        }
        else if (e.keyCode == '40') {
          // Down arrow
          if (!this.isAbleToBeMoved('down')) {
            return
          }
          newEmptyCellIndex = this.emptyCellIndex - this.squareSideLength
          oldEmptyCellIndex = this.emptyCellIndex
        }
        else if (e.keyCode == '37') {
          // Left arrow
          if (!this.isAbleToBeMoved('left')) {
            return
          }
          newEmptyCellIndex = this.emptyCellIndex + 1
          oldEmptyCellIndex = this.emptyCellIndex
        }
        else if (e.keyCode == '39') {
          // Right arrow
          if (!this.isAbleToBeMoved('right')) {
            return
          }
          newEmptyCellIndex = this.emptyCellIndex - 1
          oldEmptyCellIndex = this.emptyCellIndex
        }
        this.moveCell(newEmptyCellIndex, oldEmptyCellIndex)
        this.setEmptyCellIndex()
        this.renderKey = !this.renderKey
        if (this.isSortedArray()) {
          
        }
      }
    }
  }
</script>
<style lang="scss">
  .message {
    font-size: 2em;
    color: white;
    text-align: center;
    background: greenyellow;
    margin-bottom: 10px;
  }
  .new-game__btn {
    margin-top: 10px;
  }
</style>