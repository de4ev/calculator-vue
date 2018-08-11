<template>
    <div class="calculator">
      <div class="display">{{ current || '0' }}</div>
      <div v-if="ac" @click="clear" class="btn grey">AC</div>
      <div v-else @click="clear" class="btn grey">C</div>
      <div @click="sign" class="btn grey">+/-</div>
      <div class="btn grey">%</div>
      <div @click="divide" class="btn operator">÷</div>
      <div @click="append(7)" class="btn grey">7</div>
      <div @click="append(8)" class="btn grey">8</div>
      <div @click="append(9)" class="btn grey">9</div>
      <div @click="times" class="btn operator">×</div>
      <div @click="append(4)" class="btn grey">4</div>
      <div @click="append(5)" class="btn grey">5</div>
      <div @click="append(6)" class="btn grey">6</div>
      <div @click="minus" class="btn operator">−</div>
      <div @click="append(1)" class="btn grey">1</div>
      <div @click="append(2)" class="btn grey">2</div>
      <div @click="append(3)" class="btn grey">3</div>
      <div @click="add" class="operator btn">+</div>
      <div @click="zero" class="btn grey zero">0</div>
      <div @click="dot" class="btn grey">.</div>
      <div @click="equal" class="btn operator">=</div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      current: '',
      prev: '',
      operator: null,
      operatorClicked: false,
      currentIsAnswer: false
    }
  },
  computed: {
    ac() {
      return this.current.length === 0
    }
  },
  methods: {
    clear() {
      this.current = '';
      this.prev= '';
      this.operator= null;
      this.operatorClicked= false,
      this.currentIsAnswer= false
    },
    sign() {
      if (this.current !== '')
        this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`
    },
    append(number) {
      if (this.operatorClicked || this.currentIsAnswer) {
        this.current = '';
        this.operatorClicked = false;
        this.currentIsAnswer = false;
      }
      this.current = `${this.current}${number}`
    },
    zero() {
      if (this.currentIsAnswer) {
        this.current = '';
      } else if (this.current !== '' || this.operatorClicked) {
        this.append('0')
      }
    },
    dot() {
      if (this.current === '' || this.operatorClicked || this.currentIsAnswer) {
        this.append('0.')
      } else if (this.current.indexOf('.') === -1) {
        this.append('.')
      }
    },
    setPrevious() {
      if (this.prev !== '' && !this.operatorClicked) {
        this.equal()
      }
      this.prev = this.current;
      this.operatorClicked = true;
    },
    equal() {
      if (this.current !== '' && this.prev !== '' && !this.operatorClicked) {
        this.current = `${this.operator(
            parseFloat(this.prev),
            parseFloat(this.current)
        )}`
      }
      this.currentIsAnswer = true;
      this.prev = ''
    },
    divide() {
      this.setPrevious();
      this.operator = (a, b) => a / b;
    },
    times() {
      this.setPrevious();
      this.operator = (a, b) => a * b;
    },
    minus() {
      this.setPrevious();
      this.operator = (a, b) => a - b;
    },
    add() {
      this.setPrevious();
      this.operator = (a, b) => a + b;
    },
  }
}
</script>

<style scoped>
.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  font-size: 25px;
  box-shadow: 0 10px 40px 0;
  max-width: 250px;
  margin: 0 auto;
  border: 1px solid rgba(0, 0, 0, .4);
  border-radius: 6px;
  overflow: hidden;
}
.display {
  grid-column: 1/5;
  max-width: 250px;
  padding: 0 10px 0 10px;
  height: 70px;
  line-height: 70px;
  background: rgba(0, 0, 0, .5);
  font-size: 40px;
  text-align: right;
  color: #fff;
}
.btn {
  cursor: pointer;
  height: 50px;
  line-height: 50px;
  text-align: center;
  border: 0.5px solid rgba(0, 0, 0, .3);
}
.grey {
  background: #f2f2f2;
}
.grey:hover {
  background: #e9e9e9;
}
.operator {
  background: orange;
  color: #fff;
}
.operator:hover {
  background: #f09c00;
}
.zero {
  grid-column: 1/3;
}
</style>
