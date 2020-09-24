<template>
  <main class="main">
    <nav class="main-navigation">
      <div
        class="main-navigation__item"
        :class="{ 'active': curTabComponent === 'GetMatrix' }"
        @click="curTabComponent = 'GetMatrix'"
      >
        <p>Получить матрицу</p>
      </div>
      <div
        class="main-navigation__item"
        :class="{ 'active': curTabComponent === 'DetectLetter' }"
        @click="curTabComponent = 'DetectLetter'"
      >
        <p>Распознать букву</p>
      </div>
    </nav>
    <section class="main-body">
      <transition name="opacity-animation" mode="out-in">
        <keep-alive>
          <component :is="curTabComponentName"></component>
        </keep-alive>
      </transition>
    </section>
  </main>
</template>

<script>
  import DetectLetter from "./DetectLetter";
  import GetMatrix from "./GetMatrix";

  export default {
    components: {tabDetectLetter: DetectLetter, tabGetMatrix: GetMatrix},
    data () {
      return {
        curTabComponent: 'GetMatrix',
      };
    },
    computed: {
      curTabComponentName () {
        return `tab${this.curTabComponent}`;
      },
    },
  };
</script>

<style lang="sass">
.main
  flex-grow: 1
  display: flex
  flex-direction: column
  margin-bottom: 20px
  position: relative

.main-navigation
  width: 500px
  height: 50px
  display: flex
  align-items: stretch
  &__item
    flex-grow: 1
    display: flex
    justify-content: center
    align-items: center
    background-color: rgba(44,62,80,0.2)
    cursor: pointer
    &:hover, &.active
      background-color: rgba(44,62,80,0.4)
    &:not(:last-child)
      border-right: 2px solid #2c3e50

.main-body
  width: 100%
  box-sizing: border-box
  padding: 20px
  border-top: 1px solid rgba(44,62,80,0.2)
  border-bottom: 1px solid rgba(44,62,80,0.2)

.matrix
  margin-bottom: 20px

.opacity-animation
  &-enter-active, &-leave-active
    transition: opacity .25s ease-in-out
  &-enter-from, &-leave-to
    opacity: 0
  &-enter-to, &-leave-from
    opacity: 1
</style>