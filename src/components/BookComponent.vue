<template>
  <div class="row border py-1 my-1">
    <div class="col-1 my-auto">{{index+1}}</div>
    <div class="col-4 my-auto">{{book.nombre}}</div>
    <div class="col-4 my-auto">{{book.autor}}</div>
    <div class="col-3">
      <div
        id="btnUpdate"
        class="col-5 btn btn-success btn-sm mx-1"
        @click="() => handleUpdate($event, id)"
      ><i class="fa fa-edit"></i>
      </div>
      <div
        id="btnDelete"
        class="col-5 btn btn-danger btn-sm mx-1"
        @click="() => handleDelete($event, id)"
        ><i class="fa fa-trash"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "book-component",
  props: ["book", "id", "index"],
  methods: {
    handleDelete(event, id) {
      this.$emit("deleteBook", id)
    },
    handleUpdate(event, id) {
      this.$emit("updateBook", id)
    }
  }
}
</script>