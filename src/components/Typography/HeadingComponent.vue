<template>
  <component :is="element" class="heading">
    <slot></slot>
  </component>
</template>

<script>
export default {
  name: 'HeadingComponent',
  props: {
    element: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped lang="scss">
.heading {
  color: #475CAF;
  font-weight: 400;
  font-size: 21px;
  margin: 0;
}
</style>