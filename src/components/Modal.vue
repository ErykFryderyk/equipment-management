<template>
  <div class="overlay" @click="someEvent">
    <div class="modal">
      <!-- <button @click="showComponent('addNewScanner')">Pokaż Komponent 1</button> -->
      <!-- <button @click="showComponent('addNewPrinter')">Pokaż Komponent 2</button> -->

      <!-- Wyświetlanie odpowiedniego komponentu w zależności od aktualnego stanu -->
      <component 
        :is="component" 
        @updateData="handleUpdateData" 
        @addNewScanner="handleUpdateScanner"
        @addNewPrinter="handleUpdatePrinter"
        @removeUser="handleUpdateUsers"
        @assignDevices="handleUpdateDevicesToAssign"
        @returnDevices="handleUpdateDevicesToReturn"
      />
    </div>
  </div>
</template>

<script>
import AddNewScanner from './AddNewScanner.vue'
import AddNewPrinter from './AddNewPrinter.vue'
import AddNewUser from './AddNewUser.vue'
import RemoveUser from './RemoveUser.vue'
import AssignDevice from './AssignDevice.vue'
import ReturnDevice from './ReturnDevice.vue'

export default {
  name: 'Modal',
  components: {
    AddNewScanner,
    AddNewPrinter,
    AddNewUser,
    RemoveUser,
    AssignDevice,
    ReturnDevice,
  },
  data() {
    return {
      currentComponent: null,
    };
  },
  props: {
    component: {
      type: Object,
      required: true,
    },
  },
  methods: {
    someEvent(event) {
      const modalClass = event.target.classList.value;

      if (modalClass === 'overlay') {
        this.$emit('passEvent');
      } else {
        return;
      }
    },
    handleUpdateData(data) {
      // Otrzymujemy dane z AddNewUser i przekazujemy do komponentu rodzica
      this.$emit('updateData', data);
      // this.$emit('passEvent'); // toggle modal
    },
    handleUpdateScanner(data) {
      this.$emit('updateScanner', data);
      // this.$emit('passEvent'); // toggle modal
    },
    handleUpdatePrinter(data) {
      this.$emit('updatePrinter', data);
      // this.$emit('passEvent'); // toggle modal
    },
    handleUpdateUsers(data) {
      this.$emit('updateUsersList', data);
      // this.$emit('passEvent'); // toggle modal
    },
    handleUpdateDevicesToAssign(data){
      this.$emit('assignDevicesToUser', data);
      // Do poprawki bo nawet po błędnym wpisaniu danych zamyka modal a ponwinien posostac aktywny do ponownego wpisania danych
      // this.$emit('passEvent');  // toggle modal
    },
    // handleUpdateDevicesToReturn(data){
    //   this.$emit('returnDevices', data);
    //   // this.$emit('passEvent');
    // },
    handleUpdateDevicesToReturn(data){
      this.$emit('returnDevices', data);
      // this.$emit('passEvent'); // toggle modal
    },
    showComponent(componentName) {
      // Ustawia aktualny komponent na ten, który ma być widoczny
      this.currentComponent = componentName;
    },
  }
}
</script>

<style lang="scss" scoped>
.overlay {
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

.modal {
  width: 300px;
  min-height: 300px;
  background-color: #fff;
  box-shadow: 0 0 10px 0px #000;
  border-radius: 5px;

  @media (min-width: 520px) {
    width: 500px;
    min-height: 500px;

  }
}
</style>