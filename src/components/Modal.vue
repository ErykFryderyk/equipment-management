<template>
  <div class="overlay" @click="someEvent">
    <div class="modal">
      <button @click="showComponent('addNewScanner')">Pokaż Komponent 1</button>
      <button @click="showComponent('addNewPrinter')">Pokaż Komponent 2</button>

      <!-- Wyświetlanie odpowiedniego komponentu w zależności od aktualnego stanu -->
      <component :is="currentComponent" />
    </div>
  </div>
</template>

<script>
import addNewScanner from './addNewScanner.vue'
import addNewPrinter from './addNewPrinter.vue'

export default {
  name: 'Modal',
  components: {
    addNewScanner,
    addNewPrinter,
  },
  data() {
    return {
      currentComponent: null,
    };
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