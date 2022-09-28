<template>
  <div class="row border py-1 my-1">
    <div class="col-1">{{index+1}}</div>
    <div class="col-4">{{book.nombre}}</div>
    <div class="col-4">{{book.autor}}</div>
    <div class="col-3"></div>
  </div>
</template>

<script>
export default {
  name: "book-component",
  props: ["book", "index", "id"]
}
</script>