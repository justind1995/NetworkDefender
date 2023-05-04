<template>
    <div class="box">
        <h1>{{ positionStr }}</h1>
    </div>
</template>

<script>
export default {
  name: 'NetworkComponent',
  props: {
    positionStr: String
  }
}
</script>

<style scoped>
.box{
  width: 50px;
  height: 50px;
  border: 3px solid rgb(133, 206, 169);
  padding: 10px;
  padding-top: 10px;
  z-index: 900 !important;
}
</style>
