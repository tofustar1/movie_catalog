<template>
  <header class="header">
    <div class="container header-inner">
      <a href="#" class="logo">Movie Catalog</a>
      <input class="search" type="text" placeholder="Enter the name of the movie ">
      <span class="static-user">Alexander Borisenko</span>
    </div>
  </header>
</template>

<script setup>
</script>

<style scoped>
  .header {
    background: #BADAED;
    padding: 20px 0;
  }

  .header-inner {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo {
    font-size: 32px;
    font-weight: bold;
  }

  .search {
    flex-grow: 1;
    margin: 0 25px;
    padding: 8px 5px;
    border: none;
  }

  .static-user {
    padding: 7px 0 7px 40px;
    background: url("../../assets/icons/ic-user.png") 0 0 no-repeat;
    position: relative;
  }

  .static-user:after {
    content: '';
    position: absolute;
    top: 7px;
    right: -18px;
    background: url("../../assets/icons/ic-arrow-down.png") 0 0 no-repeat;
    width: 15px;
    height: 15px;
  }
</style>
