<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>My Test app for Vue.js 2 and Firebase</h1>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Message</h3>
      </div>
      <div class="panel-body">
        {{ newMessage}}
        <form id="form" class="form-inline" v-on:submit.prevent="addMessage">
         <div class="form-group">
            <label>Name</label>
            <input v-model="newMessage.username" class="form-control" type="text" name="author" placeholder="Name">
         </div>
          <div class="form-group">
            <label>Message</label>
            <input v-model="newMessage.message" class="form-control"  type="text" placeholder="..." name="message">
          </div>
          <button class="btn btn-primary" type="submit">Send</button>
        </form>
      </div>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Messages</h3>
      </div>
      <div class="panel-body">
        <ul class="list-group">
          <li class="list-group-item" v-for="message in messages">
            <strong>{{ message.username}}</strong>: {{ message.message}}
            <span class="glyphicon glyphicon-trash" v-on:click="removeMessage(message)"></span>
          </li>
        </ul>
      </div>
    </div>
    <!-- <img src="./assets/logo.png">
    <router-view/> -->
  </div>
</template>

<script>
import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyA6-d7pbIxwUt6DUlJjVe4gWR0DkcF7Sws",
  authDomain: "test-vue-firebase-70f0f.firebaseapp.com",
  databaseURL: "https://test-vue-firebase-70f0f.firebaseio.com",
  projectId: "test-vue-firebase-70f0f",
  storageBucket: "test-vue-firebase-70f0f.appspot.com",
  messagingSenderId: "67476265070"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let messagesRef = db.ref('messages');

export default {
  name: 'App',
  firebase: {
    messages: messagesRef
  },
  data(){
      return {
        newMessage: {
          username: "Chris",
          message: "Test"
      }
    }
  },
  methods: {
    addMessage: function(){
      messagesRef.push(this.newMessage);
      this.newMessage.message = "";
    },
    removeMessage: function(message){
      messagesRef.child(message[".key"]).remove();
      toastr.success("Message removed!");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
