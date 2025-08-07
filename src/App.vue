<script setup lang="ts">
import { ref } from 'vue'

// 词转换函数（TypeScript 版）
function wordTransform(x: string, y: string, degree: number): string {
  if (Math.random() > degree) return x;
  if (x === '，' || x === '。') return '……';
  if (x === '!' || x === '！') return '❤';
  if (x.length > 1 && Math.random() < 0.5) {
    return `${x[0]}……${x}`;
  } else {
    if (y === 'n' && Math.random() < 0.5) {
      x = '〇'.repeat(x.length);
    }
    return `……${x}`;
  }
}

// 简化分词与词性（仅演示，实际可用第三方中文分词包）
function fakeSegment(text: string): Array<[string, string]> {
  // 这里只做最简单的单字分割，名词判断用正则演示
  return Array.from(text).map(ch => {
    const isNoun = /[\u4e00-\u9fa5]/.test(ch) ? 'n' : '';
    return [ch, isNoun];
  });
}

function chs2yin(s: string, degree: number): string {
  return fakeSegment(s).map(([x, y]) => wordTransform(x, y, degree)).join('');
}

const inputText = ref('不行，那里不行。')
const degree = ref(0.5)
const outputText = ref('')

function convert() {
  outputText.value = chs2yin(inputText.value, degree.value)
}
</script>

<template>
  <div class="container">
    <h1>Yinglish 淫语体转换器</h1>
    <div class="form">
      <label for="input">输入中文：</label>
      <textarea id="input" v-model="inputText" rows="4" style="width:100%"></textarea>
      <div style="margin: 16px 0;">
        <label for="degree">淫乱度: {{ (degree * 100).toFixed(0) }}%</label>
        <input id="degree" type="range" min="0" max="1" step="0.01" v-model.number="degree" />
      </div>
      <button @click="convert" style="padding:8px 24px;">转换</button>
    </div>
    <div class="output">
      <label>输出结果：</label>
      <div class="output-box">{{ outputText }}</div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  padding: 32px;
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 2px 16px #0001;
}
.form {
  margin-bottom: 24px;
}
.output-box {
  min-height: 60px;
  background: #f7f7fa;
  border-radius: 8px;
  padding: 12px;
  font-size: 1.2em;
  margin-top: 6px;
  word-break: break-all;
}
button {
  background: #42b983;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
  margin-top: 8px;
}
button:hover {
  background: #368f6e;
}
</style>
