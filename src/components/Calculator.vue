<template>
  <div class="calculator">
    <div class="display">{{current || '0'}}</div>
    <div @click="clear" class="btn top">C</div> <!-- creates a click callback with the function 'clear'. Define function in script -->
    <div @click="sign" class="btn top">+/-</div>
    <div @click="percent" class="btn top">%</div>
    <div @click="divide" class="btn operator">÷</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="times" class="btn operator">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus" class="btn operator">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="add" class="btn operator">+</div>
    <div @click="append('0')" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn operator equal">=</div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        current: '', /* the calulator has a particular state and we are modifying that state with the methods */
        previous: null,
        operator: null,
        operatorClick: false,
      }
    },
    methods: { /* to define a function, add a methods attribute and define the method we want  */
      clear() {
        this.current = '';
      },
      sign() {
        this.current = this.current.charAt(0) === '-' ? /* if the first character is equal to '-'w */
          this.current.slice(1) : `-${this.current}`; /* return that string without the first character otherwise return the string with '-' */
      },
      percent() {
        this.current = `${parseFloat(this.current) / 100}`; /* take the current value, parse it a float, divide by 100, cast it back as a string */
      },
      append(number) {
        if (this.operatorClick) {
          this.current = '';
          this.operatorClick = false;
        }
        this.current = `${this.current}${number}`; /* plus 2 numbers using string concat */
      },
      dot() {
        if (this.current.indexOf('.') === -1) { // if the '.' does not exist. indexOf returns -1 if it cannot be found
          this.append('.'); /* apend the '.' */
        }
      },
      setPrevious() {
        this.previous = this.current;
        this.operatorClick = true;   
      },
       divide() {
        this.operator = (a, b) => a / b;
        this.setPrevious();
      },
      times() {
        this.operator = (a, b) => a * b;
        this.setPrevious();
      },
      minus() {
        this.operator = (a, b) => a - b;
        this.setPrevious();
      },
      add() {
        this.operator = (a, b) => a + b;
        this.setPrevious();
      },
      equal() {
        this.current = `${this.operator(
          parseFloat(this.previous),
          parseFloat(this.current)
        )}`;
        this.previous = null;
      }
    }
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.calculator {
  background-color: #232323;;
  width: 250px;
  height: 420px;
  margin: 0 auto;
  font-size: 28px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);/* container with 4 columns even in width */
  grid-auto-rows: minmax(50px, auto);
  padding: 24px;
  border-radius: 16px;
}

.display {
  height: 150px;
  margin-bottom: 16px;
  margin-right: 8px;
  font-size: 54px;
  display: flex;
  justify-content: flex-end;
  align-items: flex-end;
  flex-wrap: wrap;
  grid-column: 1 / 5; /* start at column and end after column 5 */
  color: #fff;
  border-bottom: 1px solid #fff;
}

.zero {
  grid-column: 1 / 3; /* this will give it 2 columns */
}

.btn {
  /* border: 1px solid #999; */
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  margin: 6px;
  border-radius: 8px;
}

.operator,
.top {
  color: #999999;
  border: none;
  font-weight: 600;
}

.equal {
  color: #f79726;
}

</style>
