<script lang="js">
import HelloWorld from './components/HelloWorld.vue';

// Declare Log decorator.
const log = (logStr) => {
  return function (target, methodName, descriptor) {
    // 保留原始方法引用
    const originalMethod = descriptor.value;

    // 重写方法
    descriptor.value = async function (...args) {
      console.log(logStr);
      originalMethod.apply(this, args);
    };

    return descriptor;
  };
};

export default {
  name: "App",
  components: {
    HelloWorld,
  },

  @log("mounted")
  mounted() {
    this.init();
  },

  methods: {
    @log("init")
    init() {},
  },
};
</script>

<template>
  <div id="app">
    <div>
      <a href="https://vitejs.dev" target="_blank">
        <img src="/vite.svg" class="logo" alt="Vite logo" />
      </a>
      <a href="https://vuejs.org/" target="_blank">
        <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
      </a>
    </div>
    <HelloWorld msg="Vite + Vue" />
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
