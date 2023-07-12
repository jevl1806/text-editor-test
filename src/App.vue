<script setup lang="ts">
import { ref } from "vue";
import TextEditor from "./components/TextEditor.vue";

enum StateCopy {
  Copying,
  Done,
  Free,
}

const value = ref("");
const stateCopy = ref(StateCopy.Free);

const handleCopy = async () => {
  stateCopy.value = StateCopy.Copying;
  await navigator.clipboard.writeText(value.value);
  stateCopy.value = StateCopy.Done;

  setTimeout(() => {
    stateCopy.value = StateCopy.Free;
  }, 1500);
};
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
    <h1>Generador de Texto Enriquecido</h1>
  </div>
  <div class="container">
    <TextEditor v-model="value" class="editor" />
    <div class="container-code">
      <button
        class="copy-btn"
        @click="handleCopy"
        :disabled="stateCopy !== StateCopy.Free"
      >
        <i v-if="stateCopy === StateCopy.Free" class="fa-solid fa-copy"></i>
        <i
          v-else-if="stateCopy === StateCopy.Copying"
          class="fa-solid fa-loader"
        ></i>
        <i v-else class="fa-solid fa-check"></i>
      </button>
      <pre class="code"><code>{{ value }}</code></pre>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.container {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.editor {
  background-color: white;
  color: black;
  flex-basis: 550px;
  flex-shrink: 0;
}

.container-code {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: black;
  padding: 2.5rem 4rem;
  min-height: 120px;
}

.code {
  flex-grow: 1;
  display: block;
  margin: 0;
  padding: 0;

  background-color: black;
}

.code code {
  overflow-wrap: break-word;
  white-space: normal;
}

.copy-btn {
  position: absolute;
  right: 0;
  top: 0;
}
</style>
