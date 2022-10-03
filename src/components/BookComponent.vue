<template>
  <div class="row border py-1 my-1">
    <div class="col-1 my-auto">{{index+1}}</div>
    <div class="col-4 my-auto">{{book.nombre}}</div>
    <div class="col-4 my-auto">{{book.autor}}</div>
    <div class="col-3">
      <div class="btn btn-danger btn-sm">Delete</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "book-component",
  props: ["book", "index", "id"]
}
</script>