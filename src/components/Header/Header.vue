<template>
  <header class="header">
    <div class="container header-inner">
      <a href="#" class="logo">Movie Catalog</a>
      <input
          v-model="localSearchQuery"
          @input="handleInput"
          class="search"
          type="search"
          placeholder="Enter the name of the movie "
      >
      <span class="static-user">Alexander Borisenko</span>
    </div>
  </header>
</template>

<script setup>
  import { ref, watch } from "vue";

  const props = defineProps({
    modelValue: String
  });

  const emits = defineEmits(['update:modelValue']);
  const localSearchQuery = ref(props.modelValue);

  watch(localSearchQuery, (newValue) => {
    emits('update:modelValue', newValue);
  });

  const handleInput = (ev) => {
    localSearchQuery.value = ev.target.value;
  };
</script>

<style scoped>
  .header {
    background: #BADAED;
    padding: 20px 0;
  }

  .header-inner {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
  }

  @media screen and (max-width: 748px) {
    .static-user {
      margin: 10px 0;
    }
  }

  .logo {
    font-size: 34px;
    font-weight: bold;
  }

  .search {
    flex-grow: 1;
    margin: 0 25px;
    padding: 6px 5px;
    border: none;
    font-size: 16px;
  }

  @media screen and (max-width: 475px) {
    .search {
      margin: 10px 0;
    }
  }

  .static-user {
    padding: 7px 18px 7px 40px;
    background: url("../../assets/icons/ic-user.png") 0 0 no-repeat;
    position: relative;
  }

  .static-user:after {
    content: '';
    position: absolute;
    top: 7px;
    right: 0;
    background: url("../../assets/icons/ic-arrow-down.png") 0 0 no-repeat;
    width: 15px;
    height: 15px;
  }
</style>
