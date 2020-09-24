<template>
  <div
    class="matrix"
    :style="{
      height: `${size}px`,
      width: `${size}px`,
    }"
  >
    <div
      class="matrix__item"
      :class="{ 'active': value[i-1] === 1}"
      v-for="i in 25"
      :key="i"
      @click="clickItem(i-1)"
    ></div>
  </div>
</template>

<script>
  export default {
    name: "Matrix",
    props: {
      size: {
        type: Number,
        default: 250
      },
      value: {
        type: Array,
        required: true
      },
    },
    methods: {
      clickItem (i) {
        const array = this.value.slice(0);
        array[i] = 1 - array[i];
        this.$emit('input', array);
      },
    },
  }
</script>

<style lang="sass">
.matrix
  display: flex
  flex-wrap: wrap
  border: 1px solid #2c3e50
  box-sizing: border-box
  &__item
    box-sizing: border-box
    border: 1px solid #2c3e50
    width: 20%
    height: 20%
    cursor: pointer
    &.active
      background-color: #2c3e50
</style>