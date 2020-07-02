<template>
  <section class="fibonacci">
    <div>
      <h1>Fibonacci Test</h1>
      <p>The max Number is the index number the Sequence will end</p>
      <p>while the Random Number is the guess of the next index number in the sequence</p>
      <form @submit.prevent="formsubmitted">
        <section class="field-box">
          <label for="max">Max Number:</label>
          <input type="number" id="max" v-model="max" />
        </section>

        <section class="field-box">
          <label for="random">Random:</label>
          <div>
            <input type="number" id="random" v-model="random" />
            <button @click.prevent="addNumber">Add</button>
          </div>
        </section>
        <section v-if="added_numbers.length != {}" class="addded_numbers">
          <div
            v-for="(status, number) of added_numbers"
            :key="number"
            class="message"
          >{{ number }} is {{status}} @ index {{ max }}</div>
        </section>
        <button type="submit">Submit</button>
      </form>
      <section class="sequence" v-if="submited">
        <h5>Sequence Generated</h5>
        <ul class="sequence__numbers">
          <li v-for="(number) of numbers" :key="number">{{ number }}</li>
        </ul>
      </section>
    </div>
  </section>
</template>

<script>
import { mapState } from "vuex";
export default {
  name: "Fibonacci",
  data() {
    return {
      max: 2,
      random: 2,
      sum: 0,
      numbers: [],
      added_numbers: {},
      valid_numbers: [],
      submited: false,
      indexes: []
    };
  },
  methods: {
    fibonacci(index, cache) {
      cache = cache || [];
      if (cache[index]) return cache[index];
      else {
        if (index < 3) return 1;
        else {
          cache[index] =
            fibonacci(index - 1, cache) + fibonacci(index - 2, cache);
        }
      }
      return cache[index];
    },
    formsubmitted() {
      //   console.log(this.series(this.max));
      this.submited = true;
      this.numbers = this.series(this.max);
    },
    series(n) {
      if (n == 0) return [0];

      if (n == 1) return [0, 1];

      let s = this.series(n - 1);
      s.push(s[s.length - 1] + s[s.length - 2]);
      return s;
    },
    addNumber() {
      console.log("max", this.max, "rand", this.random);
      this.added_numbers = {};
      let fib = this.series(this.max + 1);
      console.log(fib);
      if (fib[fib.length - 1] == this.random) {
        this.added_numbers[this.random] = "correct";
        this.indexes.push(this.random);
        // this.valid_numbers.push(this.random);
        this.max++;
        // console.log("correct array");
      } else {
        this.added_numbers[this.random] = "wrong";
        this.indexes.push(this.random);
        // console.log("wrong array");
      }
      this.random = null;
      this.numbers = this.series(this.max);
    },
    sumNumbers() {
      return this.fibonacci(this.max);
    }
  }
};
</script>

<style scoped>
.fibonacci {
  display: grid;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
form {
  margin-top: 1rem;
}
.field-box {
  display: flex;
  flex-direction: column;
  padding: 1rem;
}
.field-box div {
  width: 100%;
}
.field-box label {
  margin-bottom: 4px;
}
.field-box input,
.field-box button,
button {
  padding: 1rem;
  border: none;
  background: oldlace;
}

.field-box button,
button {
  background: rgb(241, 160, 8);
}
.sequence {
  padding: 1rem;
}

.sequence__numbers {
  list-style-type: none;
  display: flex;
  padding: 1rem;
  font-size: 1.5rem;
}
.sequence__numbers > li::after {
  content: ",";
}
.sequence__numbers > li::after,
.sequence__numbers > li::after:last-child {
  content: "";
}

.addded_numbers {
  padding: 1rem;
}
.addded_numbers ul {
  padding: 1rem;
}
.message {
  padding: 1rem;
}
</style>
