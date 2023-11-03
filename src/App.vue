<template>
  <div class="app-container">
    <h1>Warehouse Manager</h1>
    <div class="main-list">
      <div class="lists">
        <h2>Lista Użytkowników</h2>
        <ul>
          <li v-for="user in users" :key="user.userID">
            Worker: {{ user.name }} ({{ user.login }})
          </li>
        </ul>
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
import TestComponent from '@/components/TestComponent.vue';

export default {
  data() {
    return {
      components: {
        DevicesList,
        TestComponent
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
          name: 'ANIANOWAK',
        },
        {
          userID: 3,
          login: 'DARMAC',
          name: 'Darek Maciborek',
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
  }
}
</script>

<style lang="scss">
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
