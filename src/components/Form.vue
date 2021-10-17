<template>
  <div class="form">
    <header>
      <h2>Bezier curves</h2>
    </header>
    <div>
      Точек ломаной
      <select v-model="dot_number">
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
        <option value="7">7</option>
        <option value="8">8</option>
        <option value="9">9</option>
        <option value="10">10</option>
      </select>
    </div>
    <br />

    <div
      class="coord"
      v-for="counter in Number.parseInt(dot_number)"
      :key="counter"
    >
      <div>
        {{ counter }}
      </div>
      <div class="coord_item">
        x
        <input type="number" step="50" :id="`x_` + `${counter}`" value="100" />
      </div>
      <div class="coord_item">
        y
        <input type="number" step="50" value="100" :id="`y_` + `${counter}`" />
      </div>
    </div>
    <br />
    <button @click="prepare">Построить</button>
    <div class="samples">
      <button @click="sample1">Пример 1</button>
      <button @click="sample2">Пример 2</button>
      <button @click="sample3">Пример 3</button>
      <button @click="sample4">Пример 4</button>
      <button @click="sample5">Пример 5</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Form",
  emits: ["draw"],
  data() {
    return {
      dot_number: 3,
      coords: Array(),
    };
  },
  methods: {
    sample1() {
      this.dot_number = 5;
      setTimeout(() => {
        document.querySelector(`#x_1`).value = 100;
        document.querySelector(`#y_1`).value = 100;

        document.querySelector(`#x_2`).value = 400;
        document.querySelector(`#y_2`).value = 400;

        document.querySelector(`#x_3`).value = 300;
        document.querySelector(`#y_3`).value = 500;

        document.querySelector(`#x_4`).value = 200;
        document.querySelector(`#y_4`).value = 400;

        document.querySelector(`#x_5`).value = 500;
        document.querySelector(`#y_5`).value = 100;
      }, 10);
    },
    sample2() {
      this.dot_number = 5;
      setTimeout(() => {
        document.querySelector(`#x_1`).value = 100;
        document.querySelector(`#y_1`).value = 100;

        document.querySelector(`#x_2`).value = 500;
        document.querySelector(`#y_2`).value = 400;

        document.querySelector(`#x_3`).value = 300;
        document.querySelector(`#y_3`).value = 585;

        document.querySelector(`#x_4`).value = 100;
        document.querySelector(`#y_4`).value = 400;

        document.querySelector(`#x_5`).value = 500;
        document.querySelector(`#y_5`).value = 100;
      }, 10);
    },
    sample3() {
      this.dot_number = 10;
      setTimeout(() => {
        document.querySelector(`#x_1`).value = 50;
        document.querySelector(`#y_1`).value = 100;

        document.querySelector(`#x_2`).value = 550;
        document.querySelector(`#y_2`).value = 100;

        document.querySelector(`#x_3`).value = 550;
        document.querySelector(`#y_3`).value = 200;

        document.querySelector(`#x_4`).value = 50;
        document.querySelector(`#y_4`).value = 200;

        document.querySelector(`#x_5`).value = 50;
        document.querySelector(`#y_5`).value = 300;

        document.querySelector(`#x_6`).value = 550;
        document.querySelector(`#y_6`).value = 300;

        document.querySelector(`#x_7`).value = 550;
        document.querySelector(`#y_7`).value = 400;

        document.querySelector(`#x_8`).value = 50;
        document.querySelector(`#y_8`).value = 400;

        document.querySelector(`#x_9`).value = 50;
        document.querySelector(`#y_9`).value = 500;

        document.querySelector(`#x_10`).value = 550;
        document.querySelector(`#y_10`).value = 500;
      }, 0);
    },
    sample4() {
      this.dot_number = 5;
      setTimeout(() => {
        document.querySelector(`#x_1`).value = 200;
        document.querySelector(`#y_1`).value = 200;

        document.querySelector(`#x_2`).value = 400;
        document.querySelector(`#y_2`).value = 200;

        document.querySelector(`#x_3`).value = 400;
        document.querySelector(`#y_3`).value = 400;

        document.querySelector(`#x_4`).value = 200;
        document.querySelector(`#y_4`).value = 400;

        document.querySelector(`#x_5`).value = 200;
        document.querySelector(`#y_5`).value = 200;
      }, 10);
    },
    sample5() {
      this.dot_number = 9;
      setTimeout(() => {
        document.querySelector(`#x_1`).value = 300;
        document.querySelector(`#y_1`).value = 200;

        document.querySelector(`#x_2`).value = 400;
        document.querySelector(`#y_2`).value = 100;

        document.querySelector(`#x_3`).value = 450;
        document.querySelector(`#y_3`).value = 300;

        document.querySelector(`#x_4`).value = 200;
        document.querySelector(`#y_4`).value = 300;

        document.querySelector(`#x_5`).value = 300;
        document.querySelector(`#y_5`).value = 350;

        document.querySelector(`#x_6`).value = 400;
        document.querySelector(`#y_6`).value = 300;

        document.querySelector(`#x_7`).value = 150;
        document.querySelector(`#y_7`).value = 300;

        document.querySelector(`#x_8`).value = 200;
        document.querySelector(`#y_8`).value = 100;

        document.querySelector(`#x_9`).value = 300;
        document.querySelector(`#y_9`).value = 200;
      }, 10);
    },
    prepare() {
      this.dot_number = Number.parseInt(this.dot_number);
      this.coords.length = 0;
      let invalid = false;
      for (let i = 1; i < this.dot_number + 1; i++) {
        let el_x = document.querySelector(`#x_${i}`);
        let el_y = document.querySelector(`#y_${i}`);
        if (el_x.value < 0) {
          el_x.classList.add("invalid");
          invalid = true;
        } else {
          el_x.classList.remove("invalid");
        }
        if (el_y.value < 0) {
          el_y.classList.add("invalid");
          invalid = true;
        } else {
          el_y.classList.remove("invalid");
        }
        this.coords.push(new Array(el_x.value, el_y.value));
      }
      if (!invalid) {
        // console.log(this.coords);
        this.$emit("draw", this.coords);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 300px;

  padding: 10px;
  box-sizing: border-box;
  color: #fafafa;
}
.samples {
  margin-top: 10px;
}
.samples button {
  margin: 10px;
}
h2 {
  text-align: center;
  font-weight: 600;
  width: 100%;
  height: 50px;
}
.coord {
  display: flex;
  margin: 5px 0;
}
.coord .coord_item {
  background: #fafafa;
  width: 70px;
  color: #666;
}
.coord div {
  color: #fafafa;
  padding: 2px 5px;
  height: 24px;
  justify-content: center;
  border-radius: 5px;
  display: flex;
  align-items: center;
  margin-right: 10px;
}

input,
select,
button {
  border-radius: 5px;
  height: 24px;
  border: 0;
  text-align: center;
  margin-left: 5px;
  cursor: pointer;
  font-family: "Courier New", Courier, monospace;
}
button {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 24px;
  margin: 0 17px;
}
button:hover {
  cursor: pointer;
  background: #dadada;
}

input:focus,
select:focus {
  outline: none;
  border: 0;
}

input[type="number"] {
  width: 50px;
}
.invalid {
  background: #d44;
}
</style>
