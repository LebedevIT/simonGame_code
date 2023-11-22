<template>
  <article class="wrapper">
    <header>
      <h1>Simon</h1>
    </header>
    <article class="content">
      <section class="difficult">
        <h3>Cложности:</h3>
        <div class="difficult__item">
          <label for="easy">Лёгкий</label>
          <input type="radio" @input="delay = 1.5" checked name="difficult" value="easy" id="easy">
        </div>
        <div class="difficult__item">
          <label for="medium">Средний</label>
          <input type="radio" @input="delay = 1.0" name="difficult" value="medium" id="medium">
        </div>
        <div class="difficult__item">
          <label for="hard">Сложный</label>
          <input type="radio" @input="delay = 0.4" name="difficult" value="hard" id="hard">
        </div>
      </section>
      <article class="board">
        <section class="board__item n1" data-id="1" @click="handleClick"></section>
        <section class="board__item n2" data-id="2" @click="handleClick"></section>
        <section class="board__item n3" data-id="3" @click="handleClick"></section>
        <section class="board__item n4" data-id="4" @click="handleClick"></section>
      </article>
      <h3>Раундов: {{ rounds }}</h3>
    </article>
    <button @click="startGame" :disabled="isDisabled">Начать</button>
  </article>
</template>

<script>
  export default {
    data() {
      return {
        rounds: 0,
        delay: 1.5,
        melody: [],
        current_tile: 0,
        isDisabled: false,
        isActive: false
      }
    },
    methods: {
      startGame() {
        this.resetData()
        this.newRound()
        this.isDisabled = true
        this.isActive = true
      },
      newRound() {
        let tile = this.generateRandomNumber()
        this.melody.push(tile)

        if (this.melody.length > 1) {
          this.arrayDelayedIteration(1)
        } else {
          this.animate(tile)
        }
        this.rounds++
      },
      handleClick(elem) {
        const clicked = Number(elem.target.dataset.id)
        this.animate(clicked)

        if (this.current_tile <= this.melody.length && this.isActive) {
          if (this.melody[this.current_tile] == clicked) {
            this.current_tile++
            if (this.current_tile == this.melody.length) {
              this.current_tile = 0
              setTimeout(() => this.newRound(), 1000)
            } else {
              return
            }
          } else {
            this.endGame()
          }
        }
      },
      endGame() {
        alert('Вы проиграли!')
        this.resetData()
      },
      resetData() {
        this.current_tile = 0
        this.melody = []
        this.rounds = 0
        this.isDisabled = false
        this.isActive = false
      },
      arrayDelayedIteration(i) {
        if (i <= this.melody.length) {
          setTimeout(() => {
            this.animate(this.melody[i - 1])
            i++
            this.arrayDelayedIteration(i)
          }, 1000 * this.delay)
        }
      },
      animate(index) {
        const elem = document.querySelector(`.n${index}`)

        elem.classList.add('active')
        this.playSound(index)

        setTimeout(() => {
          elem.classList.remove('active')
        }, 500 * this.delay)

        setTimeout(() => {}, this.delay * 1000)
      },
      playSound(index) {
        const audio = new Audio(`/simonGame/sounds/${index}.mp3`)
        audio.play()
      },
      generateRandomNumber() {
        return Math.floor((Math.random() * 4) + 1)
      }
    }
  }
</script>

<style scoped>
  body {
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
  }

  p,
  h1,
  h2,
  h3,
  span,
  a,
  label {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    margin: 0;
    padding: 0;
  }

  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 150px;
    gap: 20px;
  }

  .wrapper header {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 20px;
  }

  .wrapper header h1 {
    font-size: 28px;
  }

  .wrapper .content {
    display: flex;
    gap: 20px;
    justify-content: center;
  }

  .wrapper .content .difficult {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .wrapper .content .board {
    display: flex;
    width: 100%;
    height: 100%;
    justify-content: center;
    flex-wrap: wrap;
    gap: 20px 20px;
    max-width: 320px;
  }

  .wrapper .content .board .board__item {
    width: 150px;
    height: 150px;
    transition: 0.5s;
  }

  .wrapper .content .board .board__item:hover {
    opacity: 0.8;
    cursor: pointer;
  }

  .wrapper .content .board .n1 {
    background-color: #00a4ef;
    box-shadow: 0 0 0 1px #00a4ef inset, 0 0 0 2px rgba(255, 255, 255, 0.15) inset, 0 8px 0 0 #0883bc, 0 8px 0 1px rgba(0, 0, 0, 0.4), 0 8px 8px 1px rgba(0, 0, 0, 0.5);
  }

  .wrapper .content .board .n1.active {
    box-shadow: 0 0 0 1px #2bbcff inset, 0 0 0 2px rgba(255, 255, 255, 0.15) inset, 0 0 0 1px rgba(0, 0, 0, 0.4);
    background-color: #2bbcff;
    transform: translateY(10px);
  }

  .wrapper .content .board .n2 {
    background-color: #f25022;
    box-shadow: 0 0 0 1px #f25022 inset, 0 0 0 2px rgba(255, 255, 255, 0.15) inset, 0 8px 0 0 #c93a12, 0 8px 0 1px rgba(0, 0, 0, 0.4), 0 8px 8px 1px rgba(0, 0, 0, 0.5);
  }

  .wrapper .content .board .n2.active {
    box-shadow: 0 0 0 1px #ff5c36 inset, 0 0 0 2px rgba(255, 255, 255, 0.15) inset, 0 0 0 1px rgba(0, 0, 0, 0.4);
    background-color: #ff5c36;
    transform: translateY(10px);
  }

  .wrapper .content .board .n3 {
    background-color: #ffb900;
    box-shadow: 0 0 0 1px #ffb900 inset, 0 0 0 2px rgba(255, 255, 255, 0.15) inset, 0 8px 0 0 #c28e07, 0 8px 0 1px rgba(0, 0, 0, 0.4), 0 8px 8px 1px rgba(0, 0, 0, 0.5);
  }

  .wrapper .content .board .n3.active {
    box-shadow: 0 0 0 1px #ffca3d inset, 0 0 0 2px rgba(255, 255, 255, 0.15) inset, 0 0 0 1px rgba(0, 0, 0, 0.4);
    background-color: #ffca3d;
    transform: translateY(10px);
  }

  .wrapper .content .board .n4 {
    background-color: #7fba00;
    box-shadow: 0 0 0 1px #7fba00 inset, 0 0 0 2px rgba(255, 255, 255, 0.15) inset, 0 8px 0 0 #638f05, 0 8px 0 1px rgba(0, 0, 0, 0.4), 0 8px 8px 1px rgba(0, 0, 0, 0.5);
  }

  .wrapper .content .board .n4.active {
    box-shadow: 0 0 0 1px #a0ea00 inset, 0 0 0 2px rgba(255, 255, 255, 0.15) inset, 0 0 0 1px rgba(0, 0, 0, 0.4);
    background-color: #a0ea00;
    transform: translateY(10px);
  }

  .wrapper button {
    padding: 15px 55px;
    border: 0;
    outline: 0;
    background-color: #649600;
    margin-top: 50px;
    color: white;
    transition: 0.3s;
  }

  .wrapper button:disabled {
    opacity: 0.5;
    filter: saturate(0);
  }

  .wrapper button:hover {
    opacity: 0.6;
  }
</style>