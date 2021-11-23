<template>
  <section>
    <header>
      <h1>My books - {{ getStatus() }} </h1>
    </header>
    <book-form @book-added='addBook'></book-form>
    <ul>
      <book-card
       v-for="book in books" :key="book.id"
       :book ='book'
       @book-remove='deleteBook'
      ></book-card>
    </ul>
  </section>
</template>

<script>
import axios from 'axios';
const BASE_REST_API_URL = process.env.VUE_APP_API_URL;
export default {
  data() {
    return {
      books: [],
    };
  },
  methods: {
    getStatus(){
      return process.env.VUE_APP_MODE;
    },
    deleteBook(bookId){
      axios
      .delete(BASE_REST_API_URL+'books/'+bookId)
      .then(()=>{
        this.books = this.books.filter(book=> book.id !== bookId);
      })
    },
    addBook(title,description){
      let newBook = {
        title: title,
        description: description,
      }
      axios
      .post(BASE_REST_API_URL+"books", newBook)
      .then( res=>{
        this.books.unshift(res.data.book);
      })
    }
  },
  mounted(){
    axios
    .get(BASE_REST_API_URL+'books')
    .then(response=>{
      this.books = response.data;
    })
    .catch(error => console.log(error))
  },
};
</script>

<style>
:root {
  --main-color: #3a2dfc;
}

* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem auto;
  border-radius: 10px;
  padding: 1rem;
  background-color: var(--main-color);
  color: white;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

button {
  cursor: pointer;
  border: 1px solid var(--main-color);
  background-color: var(--main-color);
  color: white;
  padding: 0.5rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}

button:hover,
button:active {
  box-shadow: 1px 2px 6px rgba(0, 0, 0, 0.26);
}
</style>