<template>
    <div>
        <!-- <div class="text-editor">
            <div class="editor-container">  -->
                <!-- <div class="flex flex-col h-screen"> -->
                    <div class="flex flex-row h-1_2">
              <q-input v-model="text1" label="Текст 1" type="textarea" @input="updateDiff" ref="editor1" class="w-1_2 h-full"/>
              <!-- <div class="resizer" @mousedown="startResize"></div> -->
              <q-input v-model="text2" label="Текст 2" type="textarea" @input="updateDiff" ref="editor2" class="w-1_2 h-full"/>
            </div>
        <!-- </div> -->
            <!-- </div>
        </div> -->
      <q-btn label="Сравнить" @click="compareTexts" />
      <q-btn label="Следующее различие" @click="nextDifference" />
      <div v-if="differences.length">
        <div v-for="(diff, index) in differences" :key="index" :class="{'inserted': diff.added, 'deleted': diff.removed}">
          <pre v-html="highlightDiff(diff.value)"></pre>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import { diffLines } from 'diff';
  import Prism from 'prismjs';
  import 'prismjs/themes/prism.css';
  
  export default {
    
    setup() {

      const text1 = ref('select * from dual d where d > 5');
      const text2 = ref('select * from dual d where d < 5');
      const differences = ref([]);
      let currentDiffIndex = ref(0);
  
      const compareTexts = () => {
        const diff = diffLines(text1.value, text2.value);
        differences.value = diff;
        currentDiffIndex.value = 0; // Сбросить индекс различий
      };
  
      const nextDifference = () => {
        if (differences.value.length > 0) {
          currentDiffIndex.value = (currentDiffIndex.value + 1) % differences.value.length;
          // Прокрутка к следующему различию (можно добавить логику для прокрутки)
        }
      };
  
      const highlightDiff = (text) => {
        // Подсветка синтаксиса
        return Prism.highlight(text, Prism.languages.javascript, 'javascript');
      };

      

  
      return {
        text1,
        text2,
        differences,
        compareTexts,
        nextDifference,
        highlightDiff
      };
    },
  };
  </script>
  
  <style scoped>
  .inserted {
    background-color: #d4edda;
  }
  
  .deleted {
    background-color: #f8d7da;
  }
  .editor-container {
  display: flex;
  height: 100vh;
}

.flex {
  display: flex;
}

.flex-col {
  flex-direction: column;
}

.flex-row {
  flex-direction: row;
}

.h-screen {
  height: 100vh;
}

.h-1_2 {
  height: 50vh;
}

.w-1_2 {
  width: 50%;
}
  </style>
  