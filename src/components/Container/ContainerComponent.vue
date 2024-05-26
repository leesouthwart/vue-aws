<template>
  <div class="container">
    <slot></slot>
  </div>
</template>

<script>


export default {
  name: 'ContainerComponent',
}
</script>

<style lang="scss">
.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 0 15px;

  @media (max-width: 1200px) {
    max-width: 960px;
  }

  @media (max-width: 992px) {
    max-width: 720px;
  }

  @media (max-width: 768px) {
    max-width: 540px;
  }

  @media (max-width: 576px) {
    max-width: 100%;
    padding: 0 10px;
  }
}
</style>