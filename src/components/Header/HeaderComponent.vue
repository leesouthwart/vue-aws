<template>
  <div class="header">
    <container-component>
      <div class="flex image-container">
        <img class="crop" :src="require('@/assets/spiderman.png')" alt="Spiderman">
        <heading-component element="h1">
          Gaming Habits Among Teenagers
        </heading-component>
      </div>
    </container-component>

    <div class="progress-bar"></div>
  </div>
</template>

<script>
import ContainerComponent from "@/components/Container/ContainerComponent.vue";
import HeadingComponent from "@/components/Typography/HeadingComponent.vue";

export default {
  name: 'HeaderComponent',
  components: {
    HeadingComponent,
    ContainerComponent
  }
}
</script>

<style scoped lang="scss">
.header {
  background-color: white;
  border-bottom: 4px solid #D8D8D8;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.09);
  position: relative;
}

.image-container {
  display: flex;
  align-items: center;

  .crop {
    height: 100%;
    width: auto;
    object-fit: cover;
  }

  h1 {
    font-weight: normal;
    font-size: 21px;
    color: #475CAF;
    margin: 0 0 0 20px;
  }
}

.progress-bar {
  position: absolute;
  bottom: -4px;
  width: 33%;
  height: 4px;
  background-image: linear-gradient(to right, rgba(79, 245, 172, 1), rgba(54, 147, 235, 1));
}
</style>