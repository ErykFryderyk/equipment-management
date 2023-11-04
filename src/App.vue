<template>
  <div class="app-container">
    <h1>Warehouse Manager</h1>
    <div class="lists">
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
    <div class="lists">
      <h2>Usuń użytkownika</h2>
      <form @submit.prevent="removeUserByLogin">
        <div>
          <label for="login">Login:</label>
          <input type="text" id="login" v-model="existingUser" />
        </div>
        <button type="submit">Dodaj użytkownika</button>
      </form>
    </div>
    <div class="lists">
      <h2>Wydawanie urządzeń</h2>
      <form @submit.prevent="assignDevices">
        <input type="text" v-model="userLogin" placeholder="login uzytkownika">
        <input type="text" v-model="selectedScanner" placeholder="Skaner">
        <input type="text" v-model="selectedPrinter" placeholder="Drukarka">
        <button type="submit">Przydziel urządzenie</button>
      </form>
    </div>
    <div class="lists">
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
            <b>Is In Use:</b> {{ scanner.isInUse ? 'Tak' : 'Nie' }}<br>
            <b>Model:</b> {{ scanner.model }}<br>
            <b>Serial Number:</b> {{ scanner.serialNumber }}
          </li>
        </ul>
      </div>
      <div class="lists">
        <h2>Lista Drukarek:</h2>
        <ul>
          <li v-for="printer in printers" :key="printer.printerID">
            <b>Printer ID:</b> {{ printer.printerID }}<br>
            <b>Printer Name:</b> {{ printer.printerName }}<br>
            <b>Is In Use:</b> {{ printer.isInUse ? 'Tak' : 'Nie' }}<br>
            <b>Model:</b> {{ printer.model }}<br>
            <b>Serial Number:</b> {{ printer.serialNumber }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import DevicesList from '@/components/DevicesList.vue';

export default {
  components: {
    DevicesList,
  },
  data() {
    return {
      userLogin: '', // Pole formularza - login użytkownika
      selectedScanner: null, // Pole formularza - wybrany skaner
      selectedPrinter: null, // Pole formularza - wybrana drukarka

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
      existingUser: '',
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
          scannerName: 'KON1S002',
          isInUse: false,
          model: 'TC52',
          serialNumber: 'S129281239123',
        },
        {
          scannerID: 2,
          scannerName: 'KON1S003',
          isInUse: false,
          model: 'TC52',
          serialNumber: 'S129281239123',
        },
        {
          scannerID: 3,
          scannerName: 'KON1S003',
          isInUse: false,
          model: 'TC52',
          serialNumber: 'S129281239123',
        },
      ],
      printers: [
        {
          printerID: 1,
          printerName: 'KON1L001',
          isInUse: false,
          model: 'QLn620',
          serialNumber: '123123123',
        },
        {
          printerID: 1,
          printerName: 'KON1L001',
          isInUse: false,
          model: 'QLn620',
          serialNumber: '123123123',
        },
        {
          printerID: 1,
          printerName: 'KON1L001',
          isInUse: false,
          model: 'QLn620',
          serialNumber: '123123123',
        },
      ]
    }
  },
  methods: {
    addUser() {
      // Wygeneruj nowy unikalny userID
      const maxUserID = Math.max(...this.users.map(user => user.userID));
      const newUserID = maxUserID + 1;

      // Dodaj nowego użytkownika do tablicy users
      this.users.push({
        userID: newUserID,
        login: this.newUser.login,
        name: this.newUser.name,
      });

      console.log(this.users);

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
    assignDevices() {
      // Znajdź użytkownika na podstawie loginu
      const user = this.users.find((u) => u.login === this.userLogin);
      // const scanner = this.skanners.find((u) => u.scanner === this.scannerName);

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
          this.usersWithDevices[index] = newUser;
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
