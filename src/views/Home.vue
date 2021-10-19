<template>
  <div class="home">
    <textarea @input="analyze()" v-model="inputText"></textarea>
    <table>
      <tr>
        <th>内容</th>
        <th>品詞</th>
        <th>活用</th>
      </tr>
      <tr v-for="element in output" :key="element">
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
    analyze(){
      this.kuromoji.build((err, tokenizer) => {
        const path = tokenizer.tokenize(this.inputText);
        this.output = path.filter(function(value){
          return value.pos != "助詞" && value.pos != "記号"; // 記号と助詞は不要
        });
      });
    }
  }
});
</script>
