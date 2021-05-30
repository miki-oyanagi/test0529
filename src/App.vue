<template>

  <div id="app">
    <h1>独り言app</h1>
    <div>
      <h2>つぶやく内容をコメント</h2>
      <input type="text" name='words' v-model="newWord"/>
    
      <button @click="insertWord">つぶやく</button>
    </div>
    <div>
      <h2>つぶやき一覧</h2>
      <tr v-for="item in words" :key="item.id">
        <td>{{item.id}}</td>
        <td>.{{item.words}}</td>
        <td>
        
          <button @click="deleteWord(item.id)">削除</button>
        </td>
      </tr>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data(){
    return{
      newWord:"",
      words:[],
    };
  },
  methods:{
    async getWord(){
      const resData = await axios.get("http://127.0.0.1:8001/api/");
      this.words =resData.data.data;
    },
    async insertWord(){
      const sendWord ={
        words:this.newWord,
      };
      await axios.post("http://127.0.0.1:8001/api/",sendWord);
      await this.getWord();
    },
    async deleteWord(id){
      await axios.delete("http://127.0.0.1:8001/api/" + id );
      await this.getWord();
    },
  },
  created(){
    this.getWord();
  },
};
</script>

<style>

</style>
