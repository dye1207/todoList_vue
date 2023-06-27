<template>
<nav class="navbar navbar-expand-lg bg-light">
  <div class="container-fluid">
    <router-link class="navbar-brand" :to="{ name: 'Home'}">Home</router-link>
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <router-link class="nav-link active" aria-current="page" :to="{ name: 'TodoList'}">TodoList</router-link>
        </li>
      </ul>
      <form class="d-flex" role="search">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
</nav>
<div class = "container">
  <router-view />
</div>
</template>

<script>
export default {
  setup () {
    

    return {
      serchTerm: ''
    };
  },
  methods: {
    search() {
      console.log('검색어: ', this.searchTerm);
    }
  }
};
</script>

<style>

</style>
