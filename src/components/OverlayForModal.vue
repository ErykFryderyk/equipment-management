<template>
  <div class="overlay">
    <div class="modal">

    </div>
  </div>
</template>

<script>
  export default {
    name: 'OverlayForModal',

  }
</script>

<style lang="scss" scoped>
.overlay{
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ffffffa6;
  z-index: 999;
}
.modal{
  width: 300px;
  min-height: 300px;
  background-color: #fff;
  box-shadow: 0 0 10px 0px #000;

  @media (min-width: 520px){
    width: 500px;
    min-height:500px;

  }
}
</style>