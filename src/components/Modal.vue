<template>
  <div class="overlay" @click="someEvent">
    <div class="modal">
      <ErrorAlert :class="{active: alertIsActive}">
        {{ alertText }}
      </ErrorAlert>
      <SuccessInfo :class="{active: successAlertIsActive}">
        {{ alertText }}
      </SuccessInfo>
      <!-- Wyświetlanie odpowiedniego komponentu w zależności od aktualnego stanu -->
      <component 
        :is="component"
        :successAlertRemove="successUserRemoving"
        :errorAlertRemove="failedUserRemoving"
        @updateData="handleUpdateData" 
        @addNewScanner="handleUpdateScanner"
        @addNewPrinter="handleUpdatePrinter"
        @removeUser="handleUpdateUsers"
        @assignDevices="handleUpdateDevicesToAssign"
        @returnDevices="handleUpdateDevicesToReturn"
        @alertEvent="showAlert"
        @successAlertEvent="showSuccessAlert"
        @check-this-user-name="checkThisUserName"
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
import ErrorAlert from './ErrorAlert.vue'
import SuccessInfo from './SuccessInfo.vue'

export default {
  name: 'Modal',
  components: {
    AddNewScanner,
    AddNewPrinter,
    AddNewUser,
    RemoveUser,
    AssignDevice,
    ReturnDevice,
    ErrorAlert,
    SuccessInfo
},
  data() {
    return {
      currentComponent: null,
      alertIsActive: false,
      successAlertIsActive: false,
      alertText: '',
    };
  },
  props: {
    component: {
      type: String,
    },
    successUserRemoving:{
      type: Boolean,
    },
    failedUserRemoving: {
      required: true,
    },
    // successUserAdded:{
      // type: Boolean
    // },
    successAlert: {
      type: String,
      required: true,
    },
    errorAlert: {
      type: String,
      required: true,
    }
  },
  watch: {
    successAlert(text) {
      this.showSuccessAlert(text);
      this.$emit('resetAlertStatus');
    },
    errorAlert(text) {
      this.showAlert(text);
      this.$emit('resetAlertStatus');
    },
    successUserRemoving() {},
    failedUserRemoving() {},
    // successUserAdded() {
      // this.showSuccessAlert('Nowy pracownik dodany do tablicy');
      // this.$emit('resetAlertStatus');
    // },
  },
  methods: {
    someEvent(event) {
      const modalClass = event.target.classList.value;

      if (modalClass === 'overlay') {
        this.$emit('passEvent');
        this.alertIsActive = false;
        this.successAlertIsActive = false;
      } else {
        return;
      }
    },
    showAlert(text){
      this.alertText = text;
      this.alertIsActive = true;
      setTimeout(() => {
        this.alertIsActive = false;
        // this.$emit('clearData');
      }, "4000");
    },
    showSuccessAlert(text) {
      this.alertText = text;
      this.successAlertIsActive = true;
      setTimeout(() => {
        this.successAlertIsActive = false;
        this.$emit('clearData')
      }, "4000");
    },
    handleUpdateData(data) {
      // Otrzymujemy dane z AddNewUser i przekazujemy do komponentu rodzica
      this.$emit('updateData', data);
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
    checkThisUserName(userName){
      this.$emit('userToDeleted', userName);
    }
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
  position: relative;
  width: 500px;
  min-height: 300px;
  background-color: #fff;
  box-shadow: 0 0 10px 0px #000;
  border-radius: 5px;
  padding: 30px 15px;
  overflow: hidden;

  @media (min-width: 520px) {
    width: 600px;
    min-height: 500px;
  }
}
</style>