<template>
  <div id="app">
    <h3>掲示板に投稿</h3>
    <label for="name">名前</label>
    <input id="name" type="text" v-model="name">
    <br>
    <br>
    <label for="comment">コメント</label>
    <textarea id="comment" v-model="comment"></textarea>
    <br>
    <button @click="posting">送信</button>

    <h2>掲示板</h2>
    <div v-for="post in posts" :key="post.name">
      <div>名前{{post.fields.name.stringValue}}</div>
      <div>コメント{{post.fields.comment.stringValue}}</div>
      <br><br>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data(){
    return{
      name: "",
      comment: "",
      posts: []
    };
  },
  created(){
    axios.get('https://firestore.googleapis.com/v1/projects/chat-app-ac39a/databases/(default)/documents/comment')
    .then(res => {
      this.posts = res.data.documents
    });
  },
  methods: {
    posting(){
      axios
      .post('https://firestore.googleapis.com/v1/projects/chat-app-ac39a/databases/(default)/documents/comment',{
        fields: {
          name: {
            stringValue: this.name
          },
          comment: {
            stringValue: this.comment
          }
        }
      });
      this.name = "";
      this.comment = "";
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
