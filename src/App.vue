<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Book App</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
      </div>

      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
          </div>
          <div class="form-group">
            <label for="bookUrl">URL:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books Lists</h3>
      </div>
      <div class="panel-body">
        <table class="table table-stripped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td>
                <a v-bind:href="book.url">{{book.title}}</a>
              </td>
              <td>
                {{book.author}}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span>
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
import jquery from 'jquery'

let config = {
  apiKey: "AIzaSyC0ak1cO7fCOG1DmpYMjlNa-MvTBptSCZ8",
  authDomain: "vue-book-app.firebaseapp.com",
  databaseURL: "https://vue-book-app.firebaseio.com",
  projectId: "vue-book-app",
  storageBucket: "vue-book-app.appspot.com",
  messagingSenderId: "624063912076"
}

let app = Firebase.initializeApp(config)
let db = app.database()

let booksRef = db.ref('books')

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: 'http://'
      }
    }
  },
  methods: {
    addBook: function() {
      booksRef.push(this.newBook)
      this.newBook.title = '',
      this.newBook.author = '',
      this.newBook.url = 'http://'
      toastr.success("Book added")
    },
    removeBook: function(book) {
      booksRef.child(book['.key']).remove()
      toastr.success("Book removed")
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
