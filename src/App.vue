<template>
  <div id="app" class="container">
    <!-- <img src="./assets/logo.png"> -->
    <!-- <hello></hello> -->

    <headerComponent></headerComponent>

    <div class="row">
      <div class="col-md-12">
        <div class="page-header">
          <h1>Learn Vue.js 2 & Firebase <small>from CodingTheSmartWay.com</small></h1>
        </div>
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3 class="panel-title">Add New Books</h3>
          </div>
          <div class="panel-body">
            <form id="form" class="form-inline" v-on:submit.prevent="addBook">
              <div class="form-group">
                <label for="bookTitle">Title</label>
                <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
              </div>
              <div class="form-group">
                <label for="bookAuthor">Author:</label>
                <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
              </div>
              <div class="form-group">
                <label for="bookUrl">Url:</label>
                <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
              </div>
              <input type="submit" value="Add Book" class="btn btn-primary">
            </form>
          </div>
        </div>
        
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3 class="panel-title">Book List</h3>
          </div>
          <div class="panel-body">
            <tabel class="table table-striped">
              <thead>
                <tr>
                  <th>Title</th>
                  <th>Author</th>
                  <th></th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="book in books">
                  <td><a v-bind:href="book.url">{{book.title}}</a></td>
                  <td>{{book.author}}</td>
                  <td>
                    <span class="glyphicon glyphicon-pencil" aria-hidden="true" v-on:click=""></span>
                    <span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span>
                  </td>
                </tr>
              </tbody>
            </tabel>
          </div>
        </div>
      </div>
    </div>
  
    <footerComponent></footerComponent>

  </div>
</template>

<script>
// import Hello from './components/Hello';
import Firebase from '../node_modules/firebase';
import toastr from '../node_modules/toastr';
import Header from './components/Header';
import Footer from './components/Footer';

const config = {
  apiKey: 'AIzaSyAJyqVYNXHQVIQcVo9p8WkL-nBMdP5KZ6w',
  authDomain: 'vue-firebase-296ea.firebaseapp.com',
  databaseURL: 'https://vue-firebase-296ea.firebaseio.com',
  storageBucket: 'vue-firebase-296ea.appspot.com',
  messagingSenderId: '72275570889',
  projectId: 'vue-firebase-296ea',
};

const app = Firebase.initializeApp(config);
const db = app.database();

const booksRef = db.ref('books');

export default {
  name: 'app',
  firebase: {
    books: booksRef,
  },
  data() {
    return {
      newBook: {
        title: '',
        author: '',
        url: 'http://',
      },
    };
  },

  methods: {
    addBook() {
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = 'http://';
    },
    removeBook(book) {
      booksRef.child(book['.key']).remove();
      toastr.success('Book removed successfully');
    },
  },

  components: {
    headerComponent: Header,
    footerComponent: Footer,
  },
};

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}
</style>
