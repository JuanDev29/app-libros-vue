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
            @click="handleDeployAddBook($event)"
          ></div>
        </div>
      </div>
      <div class="row border rounded bg-light py-3 my-2" v-if="deployAddBook">
        <div class="col-md-5 col-sm-12 my-1">
          <input type="text" v-model="bookName" placeholder="Nombre Libro" style="width:80%">
        </div>
        <div class="col-md-5 col-sm-12 my-1">
          <input type="text" v-model="bookAuthor" placeholder="Autor Libro" style="width:80%">
        </div>
        <div class="col-md-2 col-sm-12 my-auto">
          <div
            id='agregar'
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
        :index="index"
        :id="book.id"
      ></BookComponent>
    </div>
  </div>

</template>

<script>
import { initializeApp } from "firebase/app"
import BookComponent from './components/BookComponent.vue'
import { getFirestore, collection, getDocs, addDoc } from "firebase/firestore";

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
      deployAddBook: false,
      update: false
    }
  },
  methods: {
    async loadBooks() {
      const books = await getDocs(collection(this.db, "libros"));
      books.forEach((book) => {
        this.books.push({
          id: book.id,
          nombre: book.data().nombre,
          autor: book.data().autor
        })
      });
      console.log(this.books)
    },
    handleDeployAddBook: function(event) {
      if(event.target.id === "agregar") {
        this.deployAddBook = true
        this.button = "fa fa-minus"
        document.getElementById('agregar').setAttribute('id', 'actualizar')
      } else if (event.target.id === "actualizar") {
        this.deployAddBook = false
        this.button = "fa fa-plus"
        document.getElementById('actualizar').setAttribute('id', 'agregar')
        this.update = false
      }
    },
    handleBookForm(event) {
      if(event.target.id === "agregar") {
        const dataBook = {          
          nombre: this.bookName,
          autor: this.bookAuthor
        }
        console.log(dataBook)
        this.addBook(dataBook)        
        this.bookName = ""
        this.bookAuthor = ""
        console.log(this.books)
      }
    },
    async addBook(book) {
      let nombre = book.nombre
      let autor = book.autor
      const librosCollection = collection(this.db, "libros")
      const docRef = await addDoc(librosCollection, {
        nombre, autor
      })
      console.log(docRef.id)
      this.books.push({
        id: docRef.id, nombre, autor
      })
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
    // Initialize Firebase
    const app = initializeApp(firebaseConfig);
    console.log(app)
    this.db = getFirestore(app);
    console.log(this.db)

    this.loadBooks()
  }
}
</script>
