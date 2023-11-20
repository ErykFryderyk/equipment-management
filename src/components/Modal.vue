<template>
  <div class="overlay" @click="someEvent">
    <div class="modal">

    </div>
  </div>
</template>

<script>
  export default {
    name: 'Modal',
    methods:{
      someEvent(event){
        const modalClass = event.target.classList.value;  

        if(modalClass === 'overlay'){
          this.$emit('passEvent');
        } else {
          return;
        }
      },
    }
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
  border-radius: 5px;

  @media (min-width: 520px){
    width: 500px;
    min-height:500px;

  }
}
</style>