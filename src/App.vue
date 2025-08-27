<script lang="ts">
import {
  createDecorator,
  Component,
  Vue,
  toNative,
} from 'vue-facing-decorator';
import HelloWorld from './components/HelloWorld.vue';

function Log(prefix: string) {
  return createDecorator(
    (options, key) => {
      // Keep the original method for later.
      const originalMethod = options.methods[key];

      // Wrap the method with the logging logic.
      options.methods[key] = function wrapperMethod(...args: any[]) {
        // Print a log.
        console.log('prefix: ', prefix);
        console.log(`Invoked: ${key}(`, ...args, ')');

        // Invoke the original method.
        originalMethod.apply(this, args);
      };
    },
    {
      preserve: true, // Preserve the original method. see https://facing-dev.github.io/vue-facing-decorator/#/en/custom/custom?id=preserve
    }
  );
}

@Component({
  components: {
    HelloWorld,
  },
})
class Comp extends Vue {
  @Log('prefix')
  handleClick() {}
}

export default toNative(Comp);
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
    <button @click="handleClick">click me</button>
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
