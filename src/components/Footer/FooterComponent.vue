<template>
  <div class="footer">
    <container-component>
      <div class="flex footer-inner">
        <div class="logo">
          <img :src="require('@/assets/toluna.svg')">
        </div>

        <div class="progress">
          <button class="ghost-button"><img :src="require('@/assets/left.svg')"></button>
          <button @click="continueSubmitted" class="next-button">NEXT</button>
        </div>
      </div>
    </container-component>
  </div>
</template>

<script>
import ContainerComponent from "@/components/Container/ContainerComponent.vue";


export default {
  name: 'FooterComponent',
  components: {
    ContainerComponent
  },

  methods: {
    continueSubmitted() {
      this.$emit('continueSubmitted');
    }
  }
}
</script>

<style scoped lang="scss">
.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  padding: 16px 0;
  background-color: white;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.09);

  .footer-inner {
    display: flex;
    justify-content: space-between;
  }

  .ghost-button {
    width: 40px;
    height: 40px;
    background: linear-gradient(white, white) padding-box,
    linear-gradient(to right, rgba(79, 245, 172, 1), rgba(54, 147, 235, 1)) border-box;
    border-radius: 100%;
    border: 2px solid transparent;

    &:hover {
      cursor: pointer;
    }
  }

  .next-button {
    width: 190px;
    border-radius: 50px;
    color: white;
    font-size: 16px;
    margin-left: 1rem;
    padding: 0.5rem;
    border: none;
    background-image: linear-gradient(to right, rgba(79, 245, 172, 1), rgba(54, 147, 235, 1));

    &:hover {
      cursor: pointer;
    }
  }

}
</style>