<template>
  <div class="container">
    <Search :set-search-name="setSearchName"></Search>
    <Card :search-name="searchName"></Card>
  </div>
</template>
<script>
  import axios from 'axios';
  import Search from './Search.vue';
  import Card from './Main.vue';
  export default {
    data () {
      return {
        searchName:''
      }
    },
    components:{
      Search,
      Card
    },
    methods: {
      setSearchName (searchName) {
        this.searchName = searchName;
      }
    }
  }
</script>
<style>

</style>
