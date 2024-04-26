<template>
  <div>
    <!-- USUWANIE UŻYTKOWNIKA  -->
    <h2>Usuń użytkownika</h2>
    <form @submit.prevent="removeUserByLogin">
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': loginValid }"
          >
        </span>
        <p class="valid-text">Login musi mieć dokładnie 6 znaków (np. JANKOW)</p>
      </div>
      <div class="form-text-field" style="margin-top:20px">
        <input type="text" maxlength="6" id="login" v-model="inputValue" required="" autocomplete="off">
        <label :style="{ color: loginValid ? 'green' : 'gray' }" for="name">Login</label>
      </div>
      <div class="form-text-field">
        <input type="password" maxlength="6" id="password" v-model="passwordToRemoveUser" required="" autocomplete="off">
        <label for="name">Hasło Admina</label>
      </div>
      <button type="submit">Usuń użytkownika</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'RemoveUser',
  data() {
    return {
      errorText: 'Źle wypełnione pola w formularzu!',
      // successAlertEvent: 'Udało się usunąć chłopa!',
      inputValue: '',
      loginValid: false,
      adminPassowrd: '1234',
      passwordToRemoveUser: '1234',
      isRemoved: false,
    }
  },
  props: {
    // successAlertRemove: {
    //   Type: Boolean,
    // },
    // errorAlertRemove: {
      // Type: Boolean
    // }
  },
  watch: {
    'inputValue': function (newVal) {
      this.loginValid = newVal.length === 6 && /^[a-zA-Z]+$/.test(newVal);
    },
    // 'successAlertRemove': function() {
    //   if(this.successAlertRemove){
    //     this.showAlertInfo('successAlertEvent', 'Uzytkownika zoadsasdstał usunięty');
    //   }else{
    //     return
    //   }
    // },
    // 'errorAlertRemove': function() {
    //   if(this.errorAlertRemove){this.showAlertInfo('alertEvent', 'Uzytkownik nieistnieje!')}
    // }  
  },
  methods: {
    removeUserByLogin() {
      if (this.inputValue.length === 6 && this.adminPassowrd === this.passwordToRemoveUser) {
        this.checkingIfUserExists(this.inputValue);
      } else { 
        this.$emit('alertEvent', this.errorText);
        return;
      }
    },
    checkingIfUserExists(userName) {
      this.$emit('checkThisUserName', userName);
    },
    showAlertInfo(typeOfAlert,alertText){
      this.$emit(typeOfAlert, alertText);
    }
  },
}
</script>

<style lang="scss" scoped>
form {
  padding: 20px 100px;
  display: flex;
  justify-content: center;
  align-items: space-between;
  flex-direction: column;
}

input {
  margin-bottom: 30px;
}

.error {
  color: red;
  font-size: 12px;
}
.valid-text{
  color: #858585;
  font-size: 14px;
}
.doth{
  width: 5px;
  height: 5px;
  background-color: #a8a8a8;
}
// FORM TEXT INPUT
.form-text-field {
  width: 100%;
  position: relative;
}

.form-text-field input {
  font-size: 100%;
  padding: 0.5em;
  outline: none;
  border: 2px solid rgb(200, 200, 200);
  background-color: transparent;
  border-radius: 10px;
  width: 100%;
}

.form-text-field label {
  font-size: 100%;
  position: absolute;
  left: 0;
  padding: 0.6em;
  margin-left: 0.5em;
  pointer-events: none;
  transition: all 0.3s ease;
  color: rgb(100, 100, 100);
}

.form-text-field :is(input:focus, input:valid)~label {
  transform: translateY(-60%) scale(.9);
  margin: 0em;
  margin-left: 0.8em;
  padding: 0.4em;
  background-color: #fff;
}

.form-text-field :is(input:focus, input:valid) {
  border-color: #686868;
}

.correct{
  width: 10px;
  height: 10px;
  margin-right: 5px;
  border-radius: 50px;
  border: 1px solid gray;
  background-color: transparent;
  transition: background-color .3s ease-in-out, border .3s ease-in-out;

  &--active{
    position: relative;
    border: 1px solid gray;
    background-color: #63bd63;

    &::after{
      position: absolute;
      content: '';
      width: 10px;
      height: 10px;
      border: 1px solid green;
      border-radius: 50px;
      background-color: transparent;
      transform: translate(-50%, -50%);
      left: 50%;
      top:50%;
      animation: dissolve 0.4s ease forwards;
    }
  }

  @keyframes dissolve {
    0% {
      opacity: 100%;
    }
    100% {
      opacity: 0;
      transform: translate(-50%, -50%) scale(3);
    }
  }
}
</style>