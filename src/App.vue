<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vuejs application with Firebase database</h1>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title" />
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author" />
          </div>
          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url" />
          </div>
          <button type="submit" class="btn btn-primary">Add Book</button>
        </form>
      </div>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books List</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Title
              </th>
              <th>
                Author
              </th>
              <th>
                Delete
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books" :key="book.id">
              <td>
                <a v-bind:href="book.url" target="_blank">{{ book.title}}</a>
              </td>
              <td>
                {{ book.author }}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    
  </div>
</template>

<script>

import Firebase from 'firebase'
import toastr from 'toastr'
// create config variable with settings from firebase overview page for ex
let config = {
    apiKey: "AIzaSyDToYJ3hWtXEU6WdydcgRkZ3ykffPfA6P0",
    authDomain: "vuejs-firebase-01-59b1d.firebaseapp.com",
    databaseURL: "https://vuejs-firebase-01-59b1d.firebaseio.com",
    projectId: "vuejs-firebase-01-59b1d",
    storageBucket: "vuejs-firebase-01-59b1d.appspot.com",
    messagingSenderId: "479690171248"
}

let app = Firebase.initializeApp(config);
let db = app.database(); // create a db connection
let bookRef = db.ref('Books'); // connect to the books database

export default {
  name: 'app',
  firebase: {
    books: bookRef
  },

  data(){
    return {
      newBook: {
        title: '',
        author: '',
        url: ''

      }
    }
  },

  methods: {
    addBook: function(){
      bookRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },

    removeBook: function(book){
      bookRef.child(book['.key']).remove();
      toastr.success("Book removed!");
    }
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
