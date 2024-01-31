<template>
  <div class="app-container">
    <Modal v-show="isModalActive" @pass-event="toggleModal" :component="currentComponent" @updateData="addUser"
      @updateScanner="handleUpdateScanner" @updatePrinter="handleUpdatePrinter" @updateUsersList="handleUpdateUsers"
      @assignDevicesToUser="assignDevices" @returnDevices="returnDevices" />
    <h1>Warehouse Manager</h1>
    <button @click="toggleModal('AddNewScanner')">Dodaj skaner</button>
    <button @click="toggleModal('AddNewPrinter')">Dodaj drukarkę</button>
    <button @click="toggleModal('AddNewUser')">Dodaj uzytkownika</button>
    <button @click="toggleModal('RemoveUser')">Usuń uzytkownika</button>
    <button @click="toggleModal('AssignDevice')">Wydawanie urządzeń</button>
    <button @click="toggleModal('ReturnDevice')">Zdawanie urządzeń</button>
    <div class="main-list">
      <div class="lists">
        <input v-model="deviceToDelete" type="text" placeholder="Nazwa urządzenia">
        <button @click="removePrinter(deviceToDelete)">Usuń urządzenie</button>
        <!-- LISTA Z UŻYTKOWIKAMI KTÓRZY MAJĄ JUZ PRZYSPISANE URZĄDZENIA -->
        <h2>In use</h2>
        <div>
          <table>
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Users</th>
                <th>Printers</th>
                <th>Scanners</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(element, index) in usersWithDevices" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ element.login }}</td>
                <td>{{ element.assignedScanner }}</td>
                <td>{{ element.assignedPrinter }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

    </div>
    <div class="main-list">
      <div class="lists">
        <div class="tabs">
          <button @click="activeTab = 'users'" :class="{ 'active': activeTab === 'users' }">Użytkownicy</button>
          <button @click="activeTab = 'scanners'" :class="{ 'active': activeTab === 'scanners' }">Skanery</button>
          <button @click="activeTab = 'printers'" :class="{ 'active': activeTab === 'printers' }">Drukarki</button>
        </div>

        <div v-if="activeTab === 'users'">
          <h2>Lista użytkowników</h2>
          <table>
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Name</th>
                <th>Login</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(user, index) in users" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ user.name }}</td>
                <td>{{ user.login }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <div v-if="activeTab === 'scanners'">
          <h2>Lista skanerów</h2>
          <table>
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Name</th>
                <th>Model</th>
                <th>Serial Number</th>
                <th>Added</th>
                <th>In Use</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(scanner, index) in scanners" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ scanner.scannerName }}</td>
                <td>{{ scanner.model }}</td>
                <td>{{ scanner.serialNumber }}</td>
                <td>{{ scanner.date }}</td>
                <td>{{ scanner.isInUse ? "Tak" : "Nie" }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <div v-if="activeTab === 'printers'">
          <h2>Lista drukarek</h2>
          <table>
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Name</th>
                <th>Model</th>
                <th>Serial Number</th>
                <th>Added</th>
                <th>In Use</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(printer, index) in printers" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ printer.printerName }}</td>
                <td>{{ printer.model }}</td>
                <td>{{ printer.serialNumber }}</td>
                <td>{{ printer.date }}</td>
                <td>{{ printer.isInUse ? "Tak" : "Nie" }}</td>
              </tr>
            </tbody>
          </table>
        </div>
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
      //Testowa tabela do zakładek 
      activeTab: 'users',
      userLogin: null, // Pole formularza - login użytkownika
      selectedScanner: null, // Pole formularza - wybrany skaner
      selectedPrinter: null, // Pole formularza - wybrana drukarka
      isModalActive: false,
      existingUser: '', // zmienna do usunięcia urzytkownika
      deviceToDelete: null, // zmienna do usuwania urządzenia
      currentComponent: null, // Przechowuje aktualny widoczny komponent

      usersWithDevices: [
        {
          login: 'GRZKOS',
          assignedScanner: 'KON1S069',
          assignedPrinter: 'KON1L069',
        },
        {
          login: 'GREWIL',
          assignedScanner: 'KON1S001',
          assignedPrinter: 'KON1L001',
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
          login: 'GREWIL',
          name: 'Gregorian Wielki',
        },
        {
          userID: 5,
          login: 'GRZKOS',
          name: 'Grzegorz Kostka',
        },
      ],
      scanners: [
        {
          scannerID: 1,
          scannerName: 'KON1S001',
          isInUse: true,
          model: 'TC52',
          serialNumber: 'S129281239123',
          date: '10-10-2023',
        },
        {
          scannerID: 2,
          scannerName: 'KON1S002',
          isInUse: false,
          model: 'TC52',
          serialNumber: 'S129281239123',
          date: '10-10-2023',
        },
        {
          scannerID: 3,
          scannerName: 'KON1S069',
          isInUse: true,
          model: 'TC52',
          serialNumber: 'S129281239123',
          date: '10-10-2023',
        },
      ],
      printers: [
        {
          printerID: 1,
          printerName: 'KON1L001',
          isInUse: true,
          model: 'QLn620',
          serialNumber: '123123123',
          date: '10-10-2023',
        },
        {
          printerID: 2,
          printerName: 'KON1L002',
          isInUse: false,
          model: 'QLn620',
          serialNumber: '123123123',
          date: '10-10-2023',
        },
        {
          printerID: 3,
          printerName: 'KON1L069',
          isInUse: true,
          model: 'QLn620',
          serialNumber: '123123123',
          date: '10-10-2023',
        },
      ]
    }
  },
  methods: {
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

    addUser(data) {
      // Generowanie unikalnego userID
      const maxUserID = Math.max(...this.users.map(user => user.userID));
      const newUserID = maxUserID + 1;
      const user = this.users.find((u) => u.login === data.login);

      if (!user) {
        // Dodaj nowego użytkownika do tablicy users
        this.users.push({
          userID: newUserID,
          login: data.login,
          name: data.name,
        });
      } else {
        alert('Istnieje juz uzytkownik o takim loginie');
        return;
      }
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
          date: `${String(new Date().getDate()).padStart(2, '0')}-${String(new Date().getMonth() + 1).padStart(2, '0')}-${new Date().getFullYear()}`,
        });
        // Resetowanie danych z formularza
        this.newScanner.scannerName = '';
        this.newScanner.model = '';
        this.newScanner.serialNumber = '';
      }
    },
    removeScanner() {
      const indexToRemove = this.scanners.findIndex((scanner) => scanner.scannerName === this.deviceToDelete);
      console.log(indexToRemove);
      if (indexToRemove !== -1) {
        this.scanners.splice(indexToRemove, 1);
        this.deviceToDelete = null
      } else {
        alert('Urządzenie nie istnieje');
      }
    },
    removePrinter() {
      const indexToRemove = this.printers.findIndex((printer) => printer.printerName === this.deviceToDelete);
      if (indexToRemove !== -1) {
        this.printers.splice(indexToRemove, 1);
        this.deviceToDelete = null
      } else {
        alert('Urządzenie nie istnieje');
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
          date: `${String(new Date().getDate()).padStart(2, '0')}-${String(new Date().getMonth() + 1).padStart(2, '0')}-${new Date().getFullYear()}`,
        });
        // Resetowanie danych z formularza
        this.newPrinter.printerName = '';
        this.newPrinter.model = '';
        this.newPrinter.serialNumber = '';
      }
    },
    assignDevices(data) {
      const user = this.users.find(u => u.login === data.user);
      const scanner = this.scanners.find(u => u.scannerName === data.scanner);
      const printer = this.printers.find(u => u.printerName === data.printer);

      if (!user) {
        alert('Użytkownik nie istnieje');
        return;
      }

      const userWithDevices = this.usersWithDevices.find(u => u.login === data.user);
      if (userWithDevices) {
        alert('Użytkownik posiada już urządzenia');
        return;
      }

      if (!scanner) {
        alert('Skaner nie istnieje');
        return;
      }

      if (!printer) {
        alert('Drukarka nie istnieje');
        return;
      }

      if (scanner.isInUse || printer.isInUse) {
        alert('Urządzenia są w użyciu');
        return;
      }

      // Zmiana statusu "isInUse" w głównej tabeli
      this.scanners.find(s => s.scannerName === data.scanner).isInUse = true;
      this.printers.find(p => p.printerName === data.printer).isInUse = true;

      // Skopiowanie użytkownika i przypisanie wybranych urządzeń
      const newUser = {
        login: user.login,
        assignedScanner: scanner.scannerName,
        assignedPrinter: printer.printerName
      };

      // Dodanie użytkownika do tablicy
      this.usersWithDevices.push(newUser);

      // Zresetowanie pól formularza
      this.userLogin = '';
      this.selectedScanner = '';
      this.selectedPrinter = '';
    },
    returnDevices(data) {
      // Znajdź indeks użytkownika w tablicy usersWithDevices
      const userIndex = this.usersWithDevices.findIndex((u) => u.login === data[0]);
      const scanner = data[1];
      const printer = data[2];

      // Sprawdzanie czy istnieje uzytkownik o podanym loginie w tablicy
      if (userIndex !== -1) {
        // Sprawdzanie czy u 
        if (this.usersWithDevices[userIndex].assignedScanner === scanner && this.usersWithDevices[userIndex].assignedPrinter === printer) {

          // usuwanie użytkownika z urzadzeniami z tablicy
          this.usersWithDevices.splice(userIndex, 1);

          // Zmiana statusu "isInUse" w głównej tabeli
          this.scanners.find(s => s.scannerName === scanner).isInUse = false;
          this.printers.find(p => p.printerName === printer).isInUse = false;
        } else {
          console.error('Te urzadzenia nie są przypisane do tego uzytkownia')
        }
      } else {
        console.error('Użytkownik nie istnieje w tablicy.');
      }
    },
    //aktywowanie/dezaktywacja modal z odpowiednią zawartością
    toggleModal(componentName) {
      this.isModalActive = !this.isModalActive;
      this.currentComponent = componentName;
    },
  }
}
</script>

<style lang="scss">
/* Dodaj stylizację zakładek i treści według potrzeb */
.tabs {
  display: flex;
}

button {
  cursor: pointer;
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #ddd;
  background-color: #f2f2f2;
}

button.active {
  background-color: #ddd;
}

h2 {
  margin-bottom: 10px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 5px;
}



/* Dodaj stylizację według potrzeb */
table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

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
