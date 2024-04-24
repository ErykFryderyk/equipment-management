<template>
  <div class="app-container">
    <Modal 
      v-show="isModalActive" 
      :component="currentComponent" 
      :successAlert="successModalAlert"
      :errorAlert="errorModalAlert"
      @pass-event="toggleModal"
      @resert-alert-status="czyszczenieStatusu"
      @updateData="addUser"
      @updateScanner="addScanner" 
      @updatePrinter="addPrinter" 
      @updateUsersList="handleUpdateUsers"
      @assignDevicesToUser="assignDevices" 
      @returnDevices="returnDevices" 
      @user-to-deleted="removeUserByLogin"
      @cleanProps="cleanAlertValue"
      @clearData="emptyValueForRemoveUser"
    />
    <h1>Magazyn - Zarządzanie Urządzeniami</h1>
    <!-- <div class="buttons-box">
      <button @click="toggleModal('AssignDevice')">Wydawanie urządzeń</button>
      <button @click="toggleModal('ReturnDevice')">Zdawanie urządzeń</button>
    </div> -->
    <div class="main-list">
      <div class="lists" :class="{ 'hide-active-users-list': hideActiveUsersList }">
        <button class="wrap-button" @click="hideActiveUsersList = !hideActiveUsersList">
          <span v-if="hideActiveUsersList">+</span>
          <span v-else>-</span>
        </button>
        <!-- LISTA Z UŻYTKOWIKAMI KTÓRZY MAJĄ JUZ PRZYSPISANE URZĄDZENIA -->
        <h2>Aktywni Pracownicy</h2>
        <div class="buttons-box" v-show="!hideActiveUsersList">
          <div class="search">
            <input type="text" id="search-tabel1" v-model.trim="searchActiveUsers" required="" autocomplete="off">
            <label for="search-table1">Wyszukaj</label>
          </div>
          <button @click="toggleModal('AssignDevice')">Wydaj urządzenie</button>
          <button @click="toggleModal('ReturnDevice')">Zwróć urządzenie</button>
        </div>
        <div>
          <table v-show="!hideActiveUsersList">
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Login</th>
                <th>Skaner</th>
                <th>Drukarka</th>
                <th>Data wydania</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(element, index) in usersWithDevices" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ element.login }}</td>
                <td>{{ element.assignedScanner }}</td>
                <td>{{ element.assignedPrinter }}</td>
                <td>{{ element.date }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="main-list" :class="{ 'hide-data-lists': hideDataLists }">
      <div class="lists">
        <button class="wrap-button" @click="hideDataLists = !hideDataLists">
          <span v-if="hideDataLists">+</span>
          <span v-else>-</span>
        </button>
        <div class="tabs" v-show="!hideDataLists">
          <div class="radio-inputs">
            <label class="radio">
              <input type="radio" @click="activeTab = 'users'" name="radio" checked="">
              <span class="name">Pracownicy</span>
            </label>
            <label class="radio">
              <input type="radio" @click="activeTab = 'scanners'" name="radio">
              <span class="name">Skanery</span>
            </label>
            <label class="radio">
              <input type="radio" @click="activeTab = 'printers'" name="radio">
              <span class="name">Drukarki</span>
            </label>
            <label class="radio">
              <input type="radio" @click="activeTab = 'history'" name="radio">
              <span class="name">Historia</span>
            </label>
          </div>
        </div>

        <div v-if="activeTab === 'users'">
          <h2>Pracownicy</h2>
          <div class="search-container long-container" v-show="!hideDataLists">
            <div class="search">
              <input type="text" id="search-table2" v-model="searchUsers" required="" autocomplete="off">
              <label for="search-table2">Wyszukaj</label>
            </div>
            <button @click="toggleModal('AddNewUser')">Dodaj pracownika</button>
            <button @click="toggleModal('RemoveUser')">Usuń uzytkownika</button>
          </div>
          <table>
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Login</th>
                <th>Pracownik</th>
                <th>Opcje</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(user, index) in users" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ user.login }}</td>
                <td>{{ user.name }}</td>
                <td>
                  <button class="small-btn">Edit</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <div v-if="activeTab === 'scanners'">
          <h2>Skanery</h2>
          <div class="search-container" v-show="!hideDataLists">
            <div class="search">
              <input type="text" required="" v-model="searchScanners" autocomplete="off">
              <label for="name">Wyszukaj</label>
            </div>
            <button @click="toggleModal('AddNewScanner')">Dodaj skaner</button>
          </div>
          <table>
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Nazwa</th>
                <th>Model</th>
                <th>Numer Seryjny</th>
                <th>Dodany</th>
                <th>Gwarancja</th>
                <th>W uzyciu</th>
                <th>Serwis IT</th>
                <th>Opcje</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(scanner, index) in scanners" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ scanner.scannerName }}</td>
                <td>{{ scanner.model }}</td>
                <td>{{ scanner.serialNumber }}</td>
                <td>{{ scanner.date }}</td>
                <td>{{ scanner.warranty ? "Tak" : "Nie" }}</td>
                <td>{{ scanner.isInUse ? "Tak" : "Nie" }}</td>
                <td>{{ scanner.service ? "Tak" : "Nie" }}</td>
                <td>
                  <button class="small-btn" @click="deleteScanner(scanner.scannerID)">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <div v-if="activeTab === 'printers'">
          <h2>Drukarki</h2>
          <div class="search-container" v-show="!hideDataLists">
            <div class="search">
              <input type="text" required="" v-model="searchPrinters" autocomplete="off">
              <label for="name">Wyszukaj</label>
            </div>
            <button @click="toggleModal('AddNewPrinter')">Dodaj drukarkę</button>
          </div>
          <table>
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Nazwa</th>
                <th>Model</th>
                <th>Numer Seryjny</th>
                <th>Dodany</th>
                <th>Gwarancja</th>
                <th>W uzyciu</th>
                <th>Serwis IT</th>
                <th>Opcje</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(printer, index) in printers" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ printer.printerName }}</td>
                <td>{{ printer.model }}</td>
                <td>{{ printer.serialNumber }}</td>
                <td>{{ printer.date }}</td>
                <td>{{ printer.warranty ? "Tak" : "Nie" }}</td>
                <td>{{ printer.service ? "Tak" : "Nie" }}</td>
                <td>{{ printer.isInUse ? "Tak" : "Nie" }}</td>
                <td>
                  <button class="small-btn" @click="deletePrinter(printer.printerID)">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div v-if="activeTab === 'history'">
          <h2>Historia</h2>
          <div class="search-container long-container" v-show="!hideDataLists">
            <div class="search">
              <input type="text" id="search-table2" v-model="searchHistory" required="" autocomplete="off">
              <label for="search-table2">Wyszukaj</label>
            </div>
          </div>
          <table>
            <thead>
              <tr>
                <th style="width:20px">Lp.</th>
                <th>Login</th>
                <th>Urzadzenia</th>
                <th>W uzyciu</th>
                <th>Data</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(row, index) in historyTable" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ row.login }}</td>
                <td>{{ row.devices }}</td>
                <td>{{ row.returned ? "Nie" : "Tak"}}</td>
                <td>{{ row.date }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
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
      isModalActive: false,
      activeTab: 'users', // default value for tabs praconicy/scanery/drukarki
      existingUser: '', // zmienna do usunięcia urzytkownika
      currentComponent: null, // variable for component to show in the modal
      searchActiveUsers: '',
      searchUsers: '',
      searchScanners: '',
      searchPrinters: '',
      searchHistory: '',
      // successAlertForModal: null,
      // successRemovingAlert: null,
      errorModalAlert: '',
      successModalAlert: '',
      // errorRemovingAlert: null,
      hideActiveUsersList: false,
      hideDataLists: false,
      devices: [],

      usersWithDevices: [
        {
          login: 'GRZKOS',
          assignedScanner: 'KON1S069',
          assignedPrinter: 'KON1L069',
          date: '10-10-2024 07:50',
        },
        {
          login: 'GREWIL',
          assignedScanner: 'KON1S001',
          assignedPrinter: 'KON1L001',
          date: '10-10-2024 07:30',
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
          warranty: true,
          serviceIT: false,
          model: 'TC52',
          serialNumber: 'S129281239123',
          date: '10-10-2023',
        },
        {
          scannerID: 2,
          scannerName: 'KON1S002',
          isInUse: false,
          warranty: true,
          serviceIT: false,
          model: 'TC52',
          serialNumber: 'S129281239123',
          date: '10-10-2023',
        },
        {
          scannerID: 3,
          scannerName: 'KON1S069',
          isInUse: true,
          warranty: true,
          serviceIT: false,
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
          warranty: true,
          serviceIT: false,
          model: 'QLn620',
          serialNumber: 'XXX123123123',
          date: '10-10-2023',
        },
        {
          printerID: 2,
          printerName: 'KON1L002',
          isInUse: false,
          warranty: true,
          serviceIT: false,
          model: 'QLn620',
          serialNumber: 'XXX123123123',
          date: '10-10-2023',
        },
        {
          printerID: 3,
          printerName: 'KON1L069',
          isInUse: true,
          warranty: true,
          serviceIT: false,
          model: 'QLn620',
          serialNumber: 'XXX123123123',
          date: '10-10-2023',
        },
      ],
      historyTable: [
        {
          login: 'MARKOS',
          devices: 'KON1S070, KON1L071',
          returned: true,
          date: '10-10-2024 07:50',
        },
        {
          login: 'PAWELE',
          devices: 'KON1S090, KON1L091',
          returned: true,
          date: '10-10-2024 07:50',
        },
      ],
    }
  },
  mounted () {
    axios
      .get('https://api.coinlore.net/api/tickers/')
      .then(response => {
        this.info = response.data.data
        console.log(this.info);

      })
      .catch(err => {
        console.error(err)
      })
  },
  computed: {
    usersWithDevices() { return this.filterTable(this.usersWithDevices, this.searchActiveUsers); },
    users() { return this.filterTable(this.users, this.searchUsers); },
    scanners() { return this.filterTable(this.scanners, this.searchScanners) },
    printers() { return this.filterTable(this.printers, this.searchPrinters) },
    historyTable() { return this.filterTable(this.historyTable, this.searchHistory) },
  },
  methods: {
    //searching method 
    filterTable(table, searchValue) {
      return table.filter(tab =>
        Object.values(tab).some(value =>
          typeof value === 'string' && value.toUpperCase().includes(searchValue.toUpperCase())
        )
      );
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
        this.successModalAlert = 'Nowy uzytkownik dodany!';
      } else {
        this.errorModalAlert = 'Taki uzytkownik juz istnieje!';
        return;
      }
    },

    removeUserByLogin(name) {
      this.existingUser = name;
      const userName = this.existingUser.toUpperCase();
      const index = this.users.findIndex((u) => u.login === userName);
      const userIsWorking = this.usersWithDevices.findIndex(u => u.login === userName);


      if (userIsWorking !== -1) {
        this.errorModalAlert = 'Użytkownik nie zdał jeszcze urządzeń!';
      } else if (index === -1) {
        this.errorModalAlert = 'Nie ma takiego uzytkownika!';
      } else {
        //usuwanie z tablicy userWithDevices
        // this.successRemovingAlert = true;
        this.successModalAlert = 'Uzytkownik został usunięty!'
        this.users.splice(index, 1);
      }
    },

    emptyValueForRemoveUser(){
      this.existingUser = '';
      this.successRemovingAlert = false;
    },

    addScanner(data) {
      this.newScanner = data;
      const index = this.scanners.findIndex((elName) => elName.scannerName === this.newScanner.scannerName.toUpperCase());

      // Generowanie unikalnego userID
      const maxID = Math.max(...this.scanners.map(el => el.scannerID));
      const newID = maxID + 1;

      // Walidacja formularza
      if (this.newScanner.model === '') {
        alert("Wybierz model skanera")
      } else if (index !== -1) {
        alert("Taki skaner juz istnieje!");
        this.errorAlert = 'Taki skaner juz istnieje!'; 
        return;
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
        this.successModalAlert = 'Skaner został dodany!';

        // Resetowanie danych z formularza
        this.newScanner.scannerName = '';
        this.newScanner.model = '';
        this.newScanner.serialNumber = '';
      }
    },

    deleteScanner(scannerID) {
      if (confirm("Are you sure?")) {
        // Find the index of the scanner with the given ID
        const scanner = this.scanners.find(el => el.scannerID === scannerID);
        const index = this.scanners.findIndex(scanner => scanner.scannerID === scannerID);
        // Remove the scanner from the array
        if (index !== -1 && !scanner.isInUse) {
          this.scanners.splice(index, 1);
        } else {
          alert("Skaner jest juz w uzyciu!");
        }
      }
    },

    addPrinter(data) {
      this.newPrinter = data;
      const index = this.printers.findIndex((elName) => elName.printerName === this.newPrinter.printerName.toUpperCase());

      // Generowanie unikalnego userID
      const maxID = Math.max(...this.printers.map(el => el.printerID));
      const newID = maxID + 1;

      //Walidacja formuarza
      if (this.newPrinter.model === '') {
        alert("Wybierz model drukarki")
      } else if (index !== -1) {
        alert("Taka drukarka juz istnieje!");
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

    deletePrinter(printerID) {
      if (confirm("Are you sure?")) {
        // Find the index of the printer with the given ID
        const printer = this.printers.find(el => el.printerID === printerID);
        const index = this.printers.findIndex(printer => printer.printerID === printerID);
        // Remove the scanner from the array
        if (index !== -1 && !printer.isInUse) {
          this.printers.splice(index, 1);
        } else {
          alert("Drukarka jest juz w uzyciu!");
        }
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
        assignedPrinter: printer.printerName,
        date: `${String(new Date().getDate()).padStart(2, '0')}-${String(new Date().getMonth() + 1).padStart(2, '0')}-${new Date().getFullYear()} ${String(new Date().getHours()).padStart(2, '0')}:${String(new Date().getMinutes()).padStart(2, '0')}`,
      };

      // Dodanie użytkownika do tablicy
      this.usersWithDevices.push(newUser);

      // przekazywanie propsa do czyszczenia inputów
      // TO DO 


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
          alert('Te urzadzenia nie są przypisane do tego uzytkownia')
        }
      } else {
        alert('Użytkownik nie istnieje w tablicy.');
      }
    },

    //aktywowanie/dezaktywacja modal z odpowiednią zawartością
    toggleModal(componentName) {
      this.isModalActive = !this.isModalActive;
      this.currentComponent = componentName;
    },

    cleanAlertValue() {
      this.successRemovingAlertText = '';
      this.errorRemovingAlertText = '';
    },

    czyszczenieStatusu(){
      this.successAlertForModal = null;
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap');

*,
*::after,
*::before {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Roboto", sans-serif;
  background-color: #f2f2f2;
}

/* Dodaj stylizację zakładek i treści według potrzeb */
.tabs {
  display: flex;
}

.buttons-box {
  width: 500px;
  margin: 0 auto 15px auto;
  display: flex;
  justify-content: space-between;
}

button {
  cursor: pointer;
  border-radius: 0.5rem;
  border: 1px solid #aaa;
  background-color: #fbfbfb;
  box-sizing: border-box;
  color: rgba(51, 65, 85, 1);
  padding: 10px;
  font-size: 14px;
  font-weight: 600;
  transition: background-color .2s ease-in-out;

  &:hover {
    background-color: #e0e0e0;
  }
}

.small-btn {
  padding: 1px 6px;
  font-size: 13px;
}

.wrap-button {
  position: absolute;
  right: 15px;
  top: 10px;
}

input {
  width: 200px;
  padding: 10px;
  border: 1px solid #aaaaaa;
  background-color: #fff;
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
  font-family: "Lato", sans-serif;
}

th {
  background-color: #f2f2f2;
}

.app-container {
  text-align: center;
  padding: 20px;
}

.search-container {
  width: 340px;
  margin: 0 auto 15px auto;
  display: flex;
  align-items: center;
  justify-content: space-around;
}

.long-container {
  width: 500px;
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
  position: relative;
  flex: 1;
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 20px;
  margin: 10px;
  border-radius: 5px;

}

.hide-active-users-list,
.hide-data-lists {
  max-height: 80px;
  overflow: hidden;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 20px;
}

// TABS
.radio-inputs {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  border-radius: 0.5rem;
  background-color: #EEE;
  box-sizing: border-box;
  box-shadow: 0 0 0px 1px rgba(0, 0, 0, 0.06);
  padding: 0.25rem;
  width: 300px;
  font-size: 14px;
}

.radio-inputs .radio {
  flex: 1 1 auto;
  text-align: center;
}

.radio-inputs .radio input {
  display: none;
}

.radio-inputs .radio .name {
  display: flex;
  cursor: pointer;
  align-items: center;
  justify-content: center;
  border-radius: 0.5rem;
  border: none;
  padding: .5rem 0;
  color: rgba(51, 65, 85, 1);
  transition: all .15s ease-in-out;
}

.radio-inputs .radio input:checked+.name {
  background-color: #fff;
  font-weight: 600;
}

// SEARCH INPUT
.search {
  max-width: 190px;
  position: relative;
}

.search input {
  font-size: 100%;
  padding: 0.5em;
  outline: none;
  border: 2px solid rgb(200, 200, 200);
  background-color: transparent;
  border-radius: 10px;
  width: 100%;
}

.search label {
  font-size: 100%;
  position: absolute;
  left: 0;
  padding: 0.6em;
  margin-left: 0.5em;
  pointer-events: none;
  transition: all 0.3s ease;
  color: rgb(100, 100, 100);
}

.search :is(input:focus, input:valid)~label {
  transform: translateY(-60%) scale(.9);
  margin: 0em;
  margin-left: 0.8em;
  padding: 0.4em;
  background-color: #fff;
}

.search :is(input:focus, input:valid) {
  border-color: #686868;
}</style>
