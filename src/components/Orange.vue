<template>
  <div>
    <button @click="addOrange">リンゴを追加</button>
    <p>オレンジは{{ orange }} 個だよ</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    orange: 0
  }),
  methods: {
    addOrange: function () {
      this.orange++
      this.$emit('fluitTotal')
    }
  }
}
</script>