<template>
  <div class="row border py-1 my-1">
    <div class="col-1 my-auto">{{index+1}}</div>
    <div class="col-4 my-auto">{{book.nombre}}</div>
    <div class="col-4 my-auto">{{book.autor}}</div>
    <div class="col-3">
      <div
        class="col-5 btn btn-danger btn-sm mx-1"
        @click="() => handleDelete(id)"
      >Delete</div>
      <div
        class="col-5 btn btn-success btn-sm mx-1"
        @click="() => update(id)"
      >Update</div>
    </div>
  </div>
</template>

<script>
import { doc, deleteDoc, getDoc} from 'firebase/firestore'

export default {
  name: "book-component",
  props: ["book", "id", "index", "db"],
  methods: {
    handleDelete(id) {
      deleteDoc(doc(this.db, 'libros', id))
    },
    update(id) {
      this.getBook(id)
    },
    async getBook(id) {
      const document = await getDoc(doc(this.db, 'libros', id))
      const book = document.data()
      console.log(book)
    }
  }
}
</script>