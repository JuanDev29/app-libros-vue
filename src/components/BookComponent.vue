<template>
  <div class="row border py-1 my-1">
    <div class="col-1 my-auto">{{index+1}}</div>
    <div class="col-4 my-auto">{{book.nombre}}</div>
    <div class="col-4 my-auto">{{book.autor}}</div>
    <div class="col-3">
      <div
        class="col-5 btn btn-danger btn-sm mx-1"
        @click="handleDelete($event, id)"
      >Delete</div>
      <div
        class="col-5 btn btn-success btn-sm mx-1"
        @click="handleUpdate($event, id)"
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
    async handleDelete(e, id) {
      await deleteDoc(doc(this.db, 'libros', id))
    },
    async handleUpdate(e, id) {
      const book = await this.getBook(id)
      this.$emit("updateBook", book)
    },
    async getBook(bookId) {
      const document = await getDoc(doc(this.db, 'libros', bookId))
      return {id: bookId, ...document.data()}
    }
  }
}
</script>