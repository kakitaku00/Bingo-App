<template>
  <div>
    <input v-if="this.count === 75" type="button" v-bind:value="buttonText.reset" @click="action">
    <input v-else type="button" v-bind:value="buttonText.start" @click="action">
  </div>
</template>

<script>
export default {
  name: "bingo-button",
  props: {
    count: Number,
    buttonText: Object,
  },
  methods: {
    action: function() {
      this.$emit("action")
    }
  }
}
</script>