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
          text: "÷",
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
          text: "x",
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
    const showing = ref("");

    function buttonClicked(button) {
      if (button.type == "number") {
        showing.value += button.text;
      } else if (button.text == "C") {
        showing.value = "";
      } else if (button.text == "+/-") {
      }
    }
    return {
      buttons,
      showing,
      buttonClicked,
    };
  },
};
</script>

<style src="@/css/Styles.css">
</style>
