<template>
  <main>
    <!-- <h1>heading1</h1>
    <h2>heading2</h2> -->
    <div v-for="(line, index) in fileContent" :key="index">
      <!-- {{ line }} -->
      <TextArea
        :line="line"
        :isSelected="selectedLine === index"
        @selectLine="selectedLine = index"
      />
    </div>
    <div class="newLine">+</div>
  </main>
</template>

<script setup>
import TextArea from '@/components/TextArea.vue';
import { ref, computed, watch } from 'vue';
const props = defineProps(['file', 'reset']);
const selectedLine = ref(null);
const fileContent = computed(() => props.file.content);
window.addEventListener('keyup', (key) => {
  console.log(key);
  if (key.key === 'Escape') {
    selectedLine.value = null;
  } else if (key.key === 'ArrowDown' && key.ctrlKey) {
    selectedLine.value += 1;
  } else if (key.key === 'ArrowUp' && key.ctrlKey) {
    selectedLine.value -= 1;
  } else if (key.key === 'ArrowRight' && selectedLine.value === null) {
    selectedLine.value = 0;
  }
});

watch(
  () => props.reset,
  () => {
    selectedLine.value = null;
  }
);

// const fileContent = computed(() => props.file.content.split(/\r?\n/));

// console.log(props.file.content);
</script>

<style lang="scss" scoped>
@use '@/scss/colors.scss' as *;
main {
  color: $c-foreground;
  background-color: $c-editor;
  padding: 5%;
  height: 100%;
  * {
    margin-top: 1%;
    margin-bottom: 1%;
  }
  // overflow-x: hidden;
  overflow: scroll;
  &::-webkit-scrollbar {
    width: 2vw;
  }
  &::-webkit-scrollbar-track {
    background-color: $c-background;
    opacity: 0;
    &:hover {
      opacity: 1;
    }
  }
  &::-webkit-scrollbar-thumb {
    background-color: $c-editor;
    box-shadow: inset 0 0 4px black;

    opacity: 0;
    &:hover {
      opacity: 1;
    }
  }
}

.newLine {
  border: solid 2px $c-text-area-border;
  color: $c-text-area-border;
  font-size: 3em;
  padding: 0.5rem;
  width: 250px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  cursor: pointer;
  &:hover {
    background-color: $c-text-area;
  }
}
</style>
