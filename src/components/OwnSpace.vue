<template>
  <div class="container">
    <h2>OwnSpace</h2>
    <p>Hello, {{username}} !</p>
  </div>
</template>

<script>
export default {
  name: 'ownspace',
  data (){
    return {
      username:''
    }
  },
  mounted(){
    this.username = this.$store.state.username
  }
}
</script>
