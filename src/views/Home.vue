<template>
  <div class="home">
    <textarea @input="analyze()" v-model="inputText"></textarea>
    <table>
      <tr>
        <th>内容</th>
        <th>品詞</th>
        <th>活用</th>
      </tr>
      <tr v-for="element in output" :key="element.word_id">
        <td>{{ element.surface_form }}</td>
        <td>{{ element.pos }}</td>
        <td>{{ element.pos_detail_1 }}</td>
      </tr>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Kuromoji from 'kuromoji';

export default defineComponent({
  name: 'Home',
  data(){
    return {
      inputText: "",
      output: [] as unknown,
      kuromoji: Kuromoji.builder({dicPath: "/dict"})
    }
  },
  methods: {
    async analyze(){
      console.log("start.");
      await this.kuromoji.build((err, tokenizer) => {
        const path = tokenizer.tokenize(this.inputText);
        console.log(path);
        this.output = path;
      });
      console.log("end.");
    }
  }
});
</script>
