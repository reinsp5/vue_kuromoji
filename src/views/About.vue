<template>
  <div class="about">
    <table>
      <tr>
        <td>パッケージ名</td>
        <td>著作者名</td>
        <td>ライセンス</td>
        <td>著作権表示</td>
        <td>リポジトリ</td>
        <td>Webサイト</td>
      </tr>
      <tr v-for="license in licenses" :key="license">
        <td>{{ license.name }}</td>
        <td>{{ license.publisher }}</td>
        <td>{{ license.licenses }}</td>
        <td>{{ license.copyright }}</td>
        <td><a href="{{ license.repository }}" target="_blank" rel="noopener noreferrer">{{ license.repository }}</a></td>
        <td><a href="{{ license.url }}" target="_blank" rel="noopener noreferrer">{{ license.url }}</a></td>
      </tr>
    </table>
  </div>
</template>

<style>
table,tr,td {
  border: 1px solid black;
  border-collapse: collapse;
}
</style>

<script lang="ts">
import { defineComponent } from "vue";
import Licenses from "../assets/licenses/licenses.json";

export default defineComponent({
  name: 'about',
  data(){
    return {
      licenses: Licenses
    }
  }
});
</script>
