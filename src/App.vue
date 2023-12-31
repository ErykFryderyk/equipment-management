<template>
  <div class="app-container">
    <Modal 
      v-show="isModalActive" 
      @pass-event="toggleModal" 
      :component="currentComponent" 
      @updateData="handleUpdateData"
      @updateScanner="handleUpdateScanner"
      @updatePrinter="handleUpdatePrinter"
      @updateUsersList="handleUpdateUsers"
      @assignDevicesToUser="handleUpdateAssignedDevices"
      @returnDevices="returnDevices"
    />
    <h1>Warehouse Manager</h1>
    <button @click="toggleModal('AddNewScanner')">Dodaj skaner</button>
    <button @click="toggleModal('AddNewPrinter')">Dodaj drukarkę</button>
    <button @click="toggleModal('AddNewUser')">Dodaj uzytkownika</button>
    <button @click="toggleModal('RemoveUser')">Usuń uzytkownika</button>
    <button @click="toggleModal('AssignDevice')">Wydawanie urządzeń</button>
    <button @click="toggleModal('ReturnDevice')">Zdawanie urządzeń</button>
    <div class="main-list">
      <div class="lists">
        <!-- LISTA Z UŻYTKOWIKAMI KTÓRZY MAJĄ JUZ PRZYSPISANE URZĄDZENIA -->
        <h2>Przypisane urządzenia</h2>
        <h3 v-if="userWithDevices > 0">--brak--</h3>
        <ul>
          <li v-for="element in usersWithDevices" :key="element.userID">
            {{ element.login }} - {{ element.assignedScanner }} - {{ element.assignedPrinter }}
          </li>
        </ul>
      </div>
    </div>
    <h2>Baza Uzytkowników i urządzeń</h2>
    <div class="main-list">
      <div class="lists">
        <DevicesList :users="users" :msg="'Lista pracowników'" />
      </div>
      <div class="lists">
        <h2>Lista Skanerów:</h2>
        <ul>
          <li v-for="scanner in scanners" :key="scanner.scannerID">
            <b>Scanner ID:</b> {{ scanner.scannerID }}<br>
            <b>Scanner Name:</b> {{ scanner.scannerName }}<br>
            <b>W uzyciu:</b> {{ scanner.isInUse ? 'Tak' : 'Nie' }}<br>
            <b>Model:</b> {{ scanner.model }}<br>
            <b>Serial Number:</b> {{ scanner.serialNumber }}<br>
            <b>Ostatnio uzywany:</b> {{ scanner.recentlyActive }}
          </li>
        </ul>
      </div>
      <div class="lists">
        <h2>Lista Drukarek:</h2>
        <ul>
          <li v-for="printer in printers" :key="printer.printerID">
            <b>Printer ID:</b> {{ printer.printerID }}<br>
            <b>Printer Name:</b> {{ printer.printerName }}<br>
            <b>W uzyciu:</b> {{ printer.isInUse ? 'Tak' : 'Nie' }}<br>
            <b>Model:</b> {{ printer.model }}<br>
            <b>Serial Number:</b> {{ printer.serialNumber }}<br>
            <b>Ostatnio uzywany:</b> {{ printer.recentlyActive }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import DevicesList from '@/components/DevicesList.vue';
import Modal from '@/components/Modal.vue';

export default {
  components: {
    Modal,
    DevicesList,
  },
  data() {
    return {
      userLogin: null, // Pole formularza - login użytkownika
      selectedScanner: null, // Pole formularza - wybrany skaner
      selectedPrinter: null, // Pole formularza - wybrana drukarka
      isModalActive: false,
      existingUser: '',
      currentComponent: null, // Przechowuje aktualny widoczny komponent

      usersWithDevices: [
        {
          login: 'G',
          assignedScanner: 'KON1S111',
          assignedPrinter: 'KON1L111',
        }
      ],
      //Data to add/remove 
      newUser: {
        login: null,
        name: null,
      },
      newScanner: {
        scannerName: '',
        model: '',
        serialNumber: '',
      },
      newPrinter: {
        printerName: '',
        model: '',
        serialNumber: '',
      },
      users: [
        {
          userID: 1,
          login: 'KAMKOW',
          name: 'Kamil Kowalski',
        },
        {
          userID: 2,
          login: 'ANANOW',
          name: 'Anna Nowak',
        },
        {
          userID: 3,
          login: 'DARMAC',
          name: 'Darek Maciborek',
        },
        {
          userID: 4,
          login: 'G',
          name: 'Gregorian wielki',
        },
      ],
      scanners: [
        {
          scannerID: 1,
          scannerName: 'KON1S001',
          isInUse: false,
          model: 'TC52',
          serialNumber: 'S129281239123',
          recentlyActive: '10-10-2023',
        },
        {
          scannerID: 2,
          scannerName: 'KON1S002',
          isInUse: false,
          model: 'TC52',
          serialNumber: 'S129281239123',
          recentlyActive: '10-10-2023',
        },
      ],
      printers: [
        {
          printerID: 1,
          printerName: 'KON1L001',
          isInUse: false,
          model: 'QLn620',
          serialNumber: '123123123',
          recentlyActive: '10-10-2023',
        },
        {
          printerID: 2,
          printerName: 'KON1L002',
          isInUse: false,
          model: 'QLn620',
          serialNumber: '123123123',
          recentlyActive: '10-10-2023',
        },
      ]
    }
  },
  methods: {
    handleUpdateData(data) {
      // Obsługa zdarzenia updateData z Modal, odbieramy dane
      this.newUser = data;
      this.addUser();
    },
    handleUpdateScanner(data) {
      // Obsługa zdarzenia updateData z Modal, odbieramy dane
      this.newScanner = data;
      this.addScanner();
    },
    handleUpdatePrinter(data) {
      // Obsługa zdarzenia updateData z Modal, odbieramy dane
      this.newPrinter = data;
      this.addPrinter();
    },
    handleUpdateUsers(data) {
      // Obsługa zdarzenia updateUsers z Modal, odbieramy dane
      this.existingUser = data;
      this.removeUserByLogin();
    },
    handleUpdateAssignedDevices(data){
      this.userLogin = data.userLogin;
      this.selectedScanner = data.selectedScanner;
      this.selectedPrinter = data.selectedPrinter;
      this.assignDevices();
    },
    addUser() {
      // Generowanie unikalnego userID
      const maxUserID = Math.max(...this.users.map(user => user.userID));
      const newUserID = maxUserID + 1;

      // Dodaj nowego użytkownika do tablicy users
      this.users.push({
        userID: newUserID,
        login: this.newUser.login,
        name: this.newUser.name,
      });

      // Zresetuj dane formularza
      this.newUser.login = '';
      this.newUser.name = '';
    },
    removeUserByLogin(login) {
      const index = this.users.findIndex((u) => u.login === this.existingUser);
      if (index !== -1) {
        this.users.splice(index, 1);
      } else {
        alert('Użytkownik o podanym loginie nie istnieje.');
      }
    },
    addScanner() {
      // Generowanie unikalnego userID
      const maxID = Math.max(...this.scanners.map(user => user.scannerID));
      const newID = maxID + 1;

      // Walidacja formularza
      if (this.newScanner.model === '') {
        alert("Wybierz model skanera")
      } else {
        // Dodajwanie skanera do tablicy
        this.scanners.push({
          scannerID: newID,
          scannerName: this.newScanner.scannerName,
          isInUse: false,
          model: this.newScanner.model,
          serialNumber: this.newScanner.serialNumber,
          recentlyActive: new Date(),
        });
        // Resetowanie danych z formularza
        this.newScanner.scannerName = '';
        this.newScanner.model = '';
        this.newScanner.serialNumber = '';
      }
    },
    addPrinter() {
      // Generowanie unikalnego userID
      const maxID = Math.max(...this.printers.map(user => user.printerID));
      const newID = maxID + 1;

      //Walidacja formuarza
      if (this.newPrinter.model === '') {
        alert("Wybierz model drukarki")
      } else {
        // Dodawanie obiektu do tablicy
        this.printers.push({
          printerID: newID,
          printerName: this.newPrinter.printerName,
          isInUse: false,
          model: this.newPrinter.model,
          serialNumber: this.newPrinter.serialNumber,
          recentlyActive: new Date(),
        });
        // Resetowanie danych z formularza
        this.newPrinter.printerName = '';
        this.newPrinter.model = '';
        this.newPrinter.serialNumber = '';
      }
    },
    assignDevices() {
      // Znajdź użytkownika/skaner/drukarke na podstawie loginu
      const user = this.users.find((u) => u.login === this.userLogin);
      const scanner = this.scanners.find((u) => u.scanner === this.scannerName);
      const printer = this.printers.find((u) => u.printer === this.printerName);

      // this.isScannerNameExists();

      if (user) {
        // Skopiuj użytkownika i przypisz wybrane urządzenia
        console.log(user);
        const newUser = { ...user };
        newUser.assignedScanner = this.selectedScanner;
        newUser.assignedPrinter = this.selectedPrinter;

        console.log(newUser);
        // Dodaj użytkownika do nowej tablicy lub zaktualizuj istniejący rekord
        const index = this.usersWithDevices.findIndex((u) => u.login === user.login);
        if (index !== -1) {
          // Jeśli użytkownik jest już w tablicy, zaktualizuj go
          // this.usersWithDevices[index] = newUser;
          alert('Ten uzytkownik ma juz przypisane urzadzenia');
          return
        } else {
          // Jeśli użytkownik nie istnieje w tablicy, dodaj go
          this.usersWithDevices.push(newUser);
        }

        // Zresetuj pola formularza
        this.userLogin = '';
        this.selectedScanner = null;
        this.selectedPrinter = null;
      } else {
        alert('Użytkownik o podanym loginie nie istnieje.');
      }
      console.log(this.usersWithDevices);
    },
    returnDevices(data) {
      // Znajdź indeks użytkownika w tablicy usersWithDevices
      const index = this.usersWithDevices.findIndex((u) => u.login === data);

      if (index !== -1) {
        // Usuń użytkownika z tablicy
        this.usersWithDevices.splice(index, 1);
      } else {
        alert('Użytkownik nie istnieje w tablicy usersWithDevices.');
      }
    },
    isScannerNameExists(scannerName) {
      return this.scanners.some(scanner => scanner.scannerName === scannerName);
    },
    toggleModal(componentName){
      this.isModalActive = !this.isModalActive;
      this.currentComponent = componentName;
    }
  }
}
</script>

<style lang="scss">
body {
  background-color: #f2f2f2;
}

.app-container {
  text-align: center;
  padding: 20px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.main-list {
  display: flex;
  justify-content: space-between;
}

.lists {
  flex: 1;
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 20px;
  margin: 10px;
  border-radius: 5px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 20px;
}
</style>
