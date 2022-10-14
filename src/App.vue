<template>
  <div>
    <Header/>
    <formulario/>
  </div>
</template>

<script>
  import Header from "./components/header.vue";
  import formulario from "./components/formulario.vue";

  export default {
    components: {
      Header,
      formulario,
    }
  }
</script>
<style>
div{
  padding: 10px;
}
</style>