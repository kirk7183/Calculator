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
    const showing = ref(0);
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
        if (
          showing.value[0] == 0 &&
          firstNumber.value == 0 &&
          showing.value[1] != "."
          // showing.value.length > 1
          // showing.value[0] == "0" &&
          // showing.value[1] != "."
        ) {
          showing.value = showing.value.slice(1);
        }
        if (firstNumber.value === null) {
          firstNumber.value = Number(showing.value);
          showing.value = "";
        }
        if (button.text == "." && showing.value.includes(".")) return;
        showing.value += button.text;
      } else if (button.text == "C") {
        showing.value = "0";
        firstNumber.value = "";
        firstOperator.value = "";
      } else if (button.text == "+/-") {
        showing.value *= -1; //fast way to make it plus or minus value (2-way)
      } else if (button.text == "=") {
        equels();
        firstNumber.value = null;
        firstOperator.value = "";
      } else if (button.type == "operator") {
        if (firstOperator.value) {
          //if there is a value in firstOperator when we click on operator, then it will calculate firstNumber.value and number in showing.value
          //Example: we click on "9" and then we click on "plus" (now we have 'stored' 9 in firstNumber.value) and now we click on 1
          //(this number 1 is stored in showing.value). When we click again on operator the condition will check is there some value in
          //firstNumber and if there is it will calculate firstNumber.value and showing.value and 'store' in showing.value. After this condition
          //block is finished, it will change firstNumber.value to null and firstOperator.value to button.text
          // if (firstOperator.value === button.text) {
          //   console.log("firstOperator");
          // } else {
          // }
          equels();
        }
        firstNumber.value = null;
        firstOperator.value = button.text;
      }
    }

    function equels() {
      showing.value = operator[firstOperator.value](
        Number(firstNumber.value),
        Number(showing.value) //or we can just do + infront both values (+firstNumber.value, +showing.value) - that makes them number
      );
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
