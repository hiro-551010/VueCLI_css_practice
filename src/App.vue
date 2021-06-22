<template>
  <div class="main">
    <button @click="myAnimation='slide'">slide</button>
    <button @click="myAnimation='fade'">fade</button>
    {{ myAnimation }}
    <button @click="show = !show">切り替え</button>

    <!-- nameが無い場合はvが入ってくる -->
    <transition 
      enter-active-class="animate__animated animate__bounce"
      leave-active-class="animate__animated animate__shakeX"
      appear
    >
      <p v-if="show">hello</p>
    </transition>

    <!-- リロード時にanimationを追加するときはappear属性をつける -->
    <transition :name="myAnimation" appear>
      <div v-if="show">
        <p>bye</p>
        <p>hello</p>
        <p>hello</p>
      </div>
    </transition>

    <transition name="fade" mode="out-in">
      <p v-if="show" key="hello">こんにちは</p>
      <p v-else key="bye">さよなら</p>
    </transition>

    <br>
    <button @click="myComponent = 'componentA' ">ComponentA</button>
    <button @click="myComponent = 'componentB' ">ComponentB</button>
    <transition name="fade" mode="out-in">
      <component :is="myComponent"></component>
    </transition>
    <br>
    <!-- javascriptフックというものがある -->
    <transition
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      @enter-cancelled="enterCancelled"

      @before-leave="beforeLeave"
      @leave="leave"
      @after-leave="afterLeave"
      @leave-cancelled="leaveCancelled"
    >
      <div class="circle" v-if="show"></div>
    </transition>
    
  </div>
</template>

<script>
import ComponentA from './components/ComponentA.vue'
import ComponentB from './components/ComponentB.vue'

export default {
  components: {
    ComponentA,
    ComponentB,
  },
  data() {
    return {
      show: true,
      myAnimation: "slide",
      myComponent: 'ComponentA'
    };
  },
  methods: {
    beforeEnter() {},
    enter() {},
    afterEnter() {},
    enterCancelled() {},
    beforeLeave() {},
    leave() {},
    afterLeave() {},
    leaveCancelled() {},
  }
};
</script>

<style scoped>
.main {
  width: 70%;
  margin: auto;
  padding-top: 5rem;
  text-align: center;
}

/* transitionクラス */
.fade-enter {
  /* 現れる時の最初の状態 */
  opacity: 0;
}
.fade-enter-active {
  /* 現れる時のトランジションの状態 */
  transition: opacity 0.5s;
}
.fade-enter-to {
  /* 現れる時の最後の状態 */
  opacity: 1;
}
.fade-leave {
  /* 消える時の最初の状態 */
  opacity: 1;
}
.fade-leave-active {
  /* 消える時のトランジションの状態 */
  transition: opacity 0.5s;
}
.fade-leave-to {
  /* 消える時の最後の状態 */
  opacity: 0;
}

.slide-enter,
.slide-leave-to {
  opacity: 0;
}

.slide-enter-active {
  animation: slide-in 0.5s;
  transition: opacity 1s;
}
.slide-leave-active {
  animation: slide-in 0.5s reverse;
  transition: opacity 1s;
}

@keyframes slide-in {
  from {
    transform: translateX(100px);
  }
  to {
    transform: translateX(0)
  }
}

.circle {
  width: 200px;
  height: 200px;
  margin: auto;
  border-radius: 100px;
  background-color: deeppink;
}
</style>
