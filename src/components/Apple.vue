<template>
  <div>
    <button @click="addApple">リンゴを追加</button>
    <p>リンゴは{{ apple }} 個だよ</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    apple: 0
  }),
  methods: {
    addApple: function () {
      this.apple++
      this.$emit('fluitTotal')
    }
  }
}
</script>