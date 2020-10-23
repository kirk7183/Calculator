<template>
  <div class="main">
    <div class="screen">{{ showing }}</div>
    <div class="buttons">
      <div class="button-row" v-for="(row, index) in buttons" :key="index">
        <div
          class="button"
          :class="{ operator: button.type == 'operator' }"
          :style="button.style"
          v-for="(button, i) in row"
          :key="i"
          @click="buttonClicked(button)"
        >
          {{ button.text }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    const showing = ref("");
    const firstNumber = ref(null);
    const firstOperator = ref(null);
    const operator = {
      "/": (a, b) => a / b,
      "*": (a, b) => a * b,
      "-": (a, b) => a - b,
      "+": (a, b) => a + b,
    };

    function buttonClicked(button) {
      if (button.type == "number") {
        if (firstNumber.value === null) {
          firstNumber.value = Number(showing.value);
          showing.value = "";
        }
        showing.value += button.text;
      } else if (button.text == "C") {
        showing.value = "";
      } else if (button.text == "+/-") {
        showing.value *= -1; //fast way to make it plus or minus value (2-way)
      } else if (button.text == "=") {
        showing.value = operator[firstOperator.value](
          Number(firstNumber.value),
          Number(showing.value) //or we can just do + infront both values (+firstNumber.value, +showing.value) - that makes them number
        );
      } else if (button.type == "operator") {
        firstNumber.value = null;
        firstOperator.value = button.text;
      }
    }

    const buttons = [
      [
        {
          text: "C",
          type: "first-row",
        },
        {
          text: "+/-",
          type: "first-row",
        },
        {
          text: "%",
          type: "first-row",
        },
        {
          text: "/",
          type: "operator",
        },
      ],
      [
        {
          text: "7",
          type: "number",
        },
        {
          text: "8",
          type: "number",
        },
        {
          text: "9",
          type: "number",
        },
        {
          text: "*",
          type: "operator",
        },
      ],
      [
        {
          text: "4",
          type: "number",
        },
        {
          text: "5",
          type: "number",
        },
        {
          text: "6",
          type: "number",
        },
        {
          text: "-",
          type: "operator",
        },
      ],
      [
        {
          text: "1",
          type: "number",
        },
        {
          text: "2",
          type: "number",
        },
        {
          text: "3",
          type: "number",
        },
        {
          text: "+",
          type: "operator",
        },
      ],
      [
        {
          text: "0",
          type: "number",
        },
        {
          text: ".",
          type: "number",
          style: "flex-basis: calc(100%/2)",
        },
        {
          text: "=",
          type: "operator",
          style: "flex-basis: calc(100%/2)",
        },
      ],
    ];
    return {
      buttons,
      showing,
      buttonClicked,
      firstNumber,
      firstOperator,
    };
  },
};
</script>

<style src="@/css/Styles.css">
</style>
