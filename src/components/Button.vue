<template>
  <button @click="onClick()" :style="{ background: color }" class="btn">
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: {
      type: String,
      default: "Button",
    },
    color: {
      type: String,
      default: "black",
    },
  },
  methods: {
    onClick() {
      console.log(`Button ${this.text} clicked!`);
      this.$emit("button-click");
    },
  },
};
</script>

<style scoped></style>
