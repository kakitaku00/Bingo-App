<template>
  <div>
    <p class="view">{{target}}</p>
    <ul class="panel">
      <li v-for="num in bingo" :class="activeClass.white" :id="[num]" :key="num">{{ num }} </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "panel",
  props: {
    target: Number,
    bingo: Number,
    activeClass: Object
  },
}
</script>