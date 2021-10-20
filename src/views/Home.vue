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
    async analyze(){
      await this.kuromoji.build((err, tokenizer) => {
        const path = tokenizer.tokenize(this.inputText); // 形態素解析実行
        
        // 解析結果から助詞と記号を排除する
        const result = path.filter(function(value){
          return value.pos != "助詞" && value.pos != "記号" && value.pos != "助動詞"; // 記号と助詞は不要
        });

        // 不要な要素を削除した結果から、出現頻度リストを作成する
        const list = [] as any;
        result.forEach((element) => {
          // リストに値が存在する
          const record = list.find((v:any) => v.word_id === element.word_id);
          if(record){
            record.count += 1;
          }else{
            list.push({word_id: element.word_id, surface_form: element.surface_form, pos: element.pos, pos_detail_1: element.pos_detail_1, count: 1});
          }
        });
        console.table(list);
        this.output = result
      });
    }
  }
});
</script>
