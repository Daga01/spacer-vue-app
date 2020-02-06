<template>
<div class="outerWrapper">
  <div class="innerWrapper">
    <div class="photo">

    </div>
    <div class="description">
      <h2 class="title">Lorem ipsum</h2>
      <p class="description">
        Lorem ipsum Lorem ipsum Lorem ipsum L
        Lorem ipsum Lorem ipsum Lore Lorem ipsum
      </p>
    </div>
  </div>
  <div class="close" @click="$emit('closeModal')">
  </div>
</div>

</template>

<script>
export default {
  name: 'Modal',
};

</script>
<style lang="scss" scoped>
  .outerWrapper {
    background-color: #F6F6F6;
    height: 100%;
    max-width: 100%;
    position: fixed;
    top: 0;
    left: 0;
  }

  .close {
    position: absolute;
    right: 0;
    top: 0;
    width: 30px;
    height: 30px;
    padding: 30px;
    cursor: pointer;

    &::before, &::after {
      content: '';
      width: 20px;
      height: 2px;
      background: black;
      display: block;
      position: absolute;
      top: 30px;
      right: 20px;
    }

    &::before {
      transform: rotate(45deg);
    }

     &::after {
      transform: rotate(-45deg);
    }
  }

  .innerWrapper {
    display: flex;
    height: 100%;
    padding: 50px;
    justify-content: center;
    align-items: center;
    flex-direction: column;

    .photo {
      width: 100%;
      height: auto;
      background: black;

      img {
        width: 100%;
      }
    }

    .description {
      color: #333;
    }
  }
</style>
