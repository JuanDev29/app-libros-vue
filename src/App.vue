<template>
  <div class="container my-3">

    <div class="container my-2 py-1 text-center">
      <div class="row my-2">
        <div class="col-10 my-auto py-1">
          <h3 class="mb-0">Agregar Libro</h3>
        </div>
        <div class="col-2 my-auto py-1'">
          <div
            :class="['btn', 'btn-primary', button]"
            :id="'agregar'"
            @click="handleDeployForm($event)"
          ></div>
        </div>
      </div>
      <div class="row border rounded bg-light py-3 my-2" v-if="deployForm">
        <div class="col-md-5 col-sm-12 my-1">
          <input type="text" v-model="bookName" placeholder="Nombre Libro" style="width:80%">
        </div>
        <div class="col-md-5 col-sm-12 my-1">
          <input type="text" v-model="bookAuthor" placeholder="Autor Libro" style="width:80%">
        </div>
        <div class="col-md-2 col-sm-12 my-auto">
          <div
            v-if='update'
            id='update'
            class="btn btn-secondary"
            @click="handleBookForm($event)"
          >Actualizar</div>
          <div
            v-else
            id='add'
            class="btn btn-info"
            @click="handleBookForm($event)"
          >Agregar</div>
        </div>
      </div>
    </div>

    <div class="container my-2 py-1 text-center">
      <h3>Lista Libros</h3>
      <div class="row border rounded bg-primary py-1 my-1 text-white">
        <div class="col-1">#</div>  
        <div class="col-4">Nombre Libro</div>
        <div class="col-4">Autor Libro</div>
        <div class="col-3">Accion</div>
      </div>
      <BookComponent
        v-for="(book, index) in books"
        :key="index"
        :book="book"
        :id="book.id"
        :index="index"
        :db="this.db"
        @updateBook="updateBook($event)"
      ></BookComponent>
    </div>
  </div>

</template>

<script>
import { initializeApp } from "firebase/app"
import BookComponent from './components/BookComponent.vue'
import { getFirestore, collection, addDoc, onSnapshot, doc, updateDoc } from "firebase/firestore";

export default {
  name: 'App',
  components: {
    BookComponent
  },
  data: function() {
    return {
      db: "",
      books: [],
      bookName: "",
      bookAuthor: "",
      button: "fa fa-plus",
      deployForm: false,
      update: false,
      bookId: ""
    }
  },
  methods: {
    loadBooks() {
      onSnapshot(collection(this.db, "libros"), (querySnapshot) => {
        const docs = []
        querySnapshot.forEach(doc => {
          docs.push({id: doc.id, ...doc.data()})
        })
        this.books = docs
      })
    },
    handleDeployForm: function(event) {
      if(event.target.id === "agregar") {
        this.deployForm = true
        this.button = "fa fa-minus"
        document.getElementById('agregar').setAttribute('id', 'actualizar')
      } else if (event.target.id === "actualizar") {
        this.deployForm = false
        this.button = "fa fa-plus"
        document.getElementById('actualizar').setAttribute('id', 'agregar')
        this.update = false
      }
    },
    handleBookForm(event) {
      if(event.target.id === "add") {
        const bookData = {
          nombre: this.bookName,
          autor: this.bookAuthor
        }
        this.addBook(bookData)
        this.bookName = ""
        this.bookAuthor = ""
        this.deployForm = false
        this.button = "fa fa-plus"
        //document.getElementById("update").setAttribute("id", "add");
        console.log(this.books)
      }
      else if(event.target.id === "update") {
        //this.updateBook()
        //document.getElementById("add").setAttribute("id", "update");
        const bookData = {
          nombre: this.bookName,
          autor: this.bookAuthor
        }
        console.log(bookData)
        updateDoc(doc(this.db, 'libros', this.bookId), bookData)
        this.update = false;
        this.deployForm = false
        this.button = "fa fa-plus"
        this.bookName = ""
        this.bookAuthor = ""
      }
    },
    async addBook(book) {
      let nombre = book.nombre
      let autor = book.autor
      await addDoc(collection(this.db, "libros"), {nombre, autor})
    },
    async updateBook(book) {
      console.log(book)
      this.deployForm = true
      this.update = true
      this.bookAuthor = book.autor
      this.bookName = book.nombre
      this.bookId = book.id
      this.button = "fa fa-minus"
      console.log(this.bookId)
    }
  },
  beforeMount() {
    const firebaseConfig = {
      apiKey: "AIzaSyAplxcMtg0eNqmTRzOInSJ35S5vt3hxHZ8",
      authDomain: "applibros-vue.firebaseapp.com",
      projectId: "applibros-vue",
      storageBucket: "applibros-vue.appspot.com",
      messagingSenderId: "844111479312",
      appId: "1:844111479312:web:8f7e1b2614acc9d2490095"
    };
    const app = initializeApp(firebaseConfig); // Initialize Firebase
    this.db = getFirestore(app); // Initialize Firestore
    console.log(this.db)
    this.loadBooks()
  },
}
</script>
