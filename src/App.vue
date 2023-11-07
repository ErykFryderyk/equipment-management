<template>
  <div class="app-container">
    <OverlayForModal v-show="modalIsActive"/>
    <h1>Warehouse Manager</h1>
    <div class="lists">
      <!-- DODAWANIE NOWEGO UŻYTKOWNIKA -->
      <h2>Dodaj nowego użytkownika</h2>
      <form @submit.prevent="addUser">
        <div>
          <label for="login">Login:</label>
          <input type="text" id="login" v-model="newUser.login" />
        </div>
        <div>
          <label for="name">Imię i nazwisko:</label>
          <input type="text" id="name" v-model="newUser.name" />
        </div>
        <button type="submit">Dodaj użytkownika</button>
      </form>
    </div>
    <!-- USUWANIE UŻYTKOWNIKA  -->
    <div class="lists">
      <h2>Usuń użytkownika</h2>
      <form @submit.prevent="removeUserByLogin">
        <div>
          <label for="login">Login:</label>
          <input type="text" id="login" v-model="existingUser" />
        </div>
        <button type="submit">Usuń użytkownika</button>
      </form>
    </div>
    <div class="lists">
      <!-- DODAWANIE SKANERA DO BAZY -->
      <h2>Dodaj skaner</h2>
      <form @submit.prevent="addScanner">
        <div>
          <label for="scannerName">Scanner Name:</label>
          <input type="text" id="scannerName" maxlength="8" placeholder="KON1S001" v-model="newScanner.scannerName" />
        </div>
        <div>
          <label for="model">Model:</label>
          <select id="model" v-model="newScanner.model">
            <option value="">--brak--</option>
            <option value="TC52">TC52</option>
            <option value="MC33">MC33</option>
            <option value="MC55">MC55</option>
            <option value="MC67">MC67</option>
          </select>
        </div>
        <div>
          <label for="scannerSerialNumber">Serial Number:</label>
          <input type="text" id="scannerSerialNumber" placeholder="S7265638782641" v-model="newScanner.serialNumber" />
        </div>
        <button type="submit">Dodaj skaner</button>
      </form>
    </div>
    <div class="lists">
      <!-- DODAWANIE DRUKARKI DO BAZY -->
      <h2>Dodaj drukarkę</h2>
      <form @submit.prevent="addPrinter">
        <div>
          <label for="printerName">Printer Name:</label>
          <input type="text" id="printerName" placeholder="KON1L001" v-model="newPrinter.printerName" />
        </div>
        <div>
          <label for="model">Model:</label>
          <select id="model" v-model="newPrinter.model">
            <option value="">--brak--</option>
            <option value="QLn620">QLn620</option>
            <option value="QLn220">QLn220</option>
            <option value="ZQ630">ZQ630</option>
          </select>
        </div>
        <div>
          <label for="printerSerialNumber">Serial Number:</label>
          <input type="text" id="printerSerialNumber" placeholder="S1203120312031" v-model="newPrinter.serialNumber" />
        </div>
        <button type="submit">Dodaj skaner</button>
      </form>
    </div>
    <div class="lists">
      <!-- WYDAWANIE SPRZĘTU -->
      <h2>Wydawanie urządzeń</h2>
      <form @submit.prevent="assignDevices">
        <input type="text" v-model="userLogin" placeholder="login uzytkownika">
        <input type="text" v-model="selectedScanner" placeholder="Skaner">
        <input type="text" v-model="selectedPrinter" placeholder="Drukarka">
        <button type="submit">Przydziel urządzenie</button>
      </form>
    </div>
    <div class="lists">
      <!-- ZDAWANIE SPRZĘTU -->
      <h2>Zdawanie urządzeń</h2>
      <form @submit.prevent="returnDevices">
        <input type="text" placeholder="login uzytkownika">
        <input type="text" placeholder="Skaner">
        <input type="text" placeholder="Drukarka">
        <button type="submit">Przydziel urządzenie</button>
      </form>
    </div>
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
import OverlayForModal from '@/components/OverlayForModal.vue';

export default {
  components: {
    OverlayForModal,
    DevicesList,
  },
  data() {
    return {
      userLogin: '', // Pole formularza - login użytkownika
      selectedScanner: null, // Pole formularza - wybrany skaner
      selectedPrinter: null, // Pole formularza - wybrana drukarka
      modalIsActive: false,
      existingUser: '',

      usersWithDevices: [
        // {
        //   userLogin: '',
        //   assignedScanner: null,
        //   assignedPrinter: null,
        // }
      ],
      newUser: {
        login: '',
        name: '',
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
      // Znajdź użytkownika na podstawie loginu
      const user = this.users.find((u) => u.login === this.userLogin);
      const scanner = this.scanners.find((u) => u.scanner === this.scannerName);
      const printer = this.printers.find((u) => u.printer === this.printerName);

      // this.isScannerNameExists();

      if (user) {
        // Skopiuj użytkownika i przypisz wybrane urządzenia
        const newUser = { ...user };
        newUser.assignedScanner = this.selectedScanner;
        newUser.assignedPrinter = this.selectedPrinter;

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
    returnDevice(user) {
      // Znajdź indeks użytkownika w tablicy usersWithDevices
      const index = this.usersWithDevices.findIndex(u => u === user);

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
