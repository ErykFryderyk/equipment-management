<template>
    <div class="error">
      <div class="error__icon">
        <svg fill="none" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg">
          <path
            d="m13 13h-2v-6h2zm0 4h-2v-2h2zm-1-15c-1.3132 0-2.61358.25866-3.82683.7612-1.21326.50255-2.31565 1.23915-3.24424 2.16773-1.87536 1.87537-2.92893 4.41891-2.92893 7.07107 0 2.6522 1.05357 5.1957 2.92893 7.0711.92859.9286 2.03098 1.6651 3.24424 2.1677 1.21325.5025 2.51363.7612 3.82683.7612 2.6522 0 5.1957-1.0536 7.0711-2.9289 1.8753-1.8754 2.9289-4.4189 2.9289-7.0711 0-1.3132-.2587-2.61358-.7612-3.82683-.5026-1.21326-1.2391-2.31565-2.1677-3.24424-.9286-.92858-2.031-1.66518-3.2443-2.16773-1.2132-.50254-2.5136-.7612-3.8268-.7612z"
            fill="#393a37"></path>
        </svg>
      </div>
      <div class="error__title">
        <slot>
          There is a error!
        </slot>
      </div>
    </div>
</template>

<script>
export default {
  name: "ErrorAlert",
  data() {
    return {}
  }
}
</script>

<style lang="scss" scoped>
.error {
  position: absolute;
  z-index: 999;
  left: 50%;
  transform: translate(-50%, -200%);
  width: 320px;
  padding: 12px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: start;
  background: #FCE8DB;
  border-radius: 8px;
  border: 1px solid #EF665B;
  box-shadow: 0px 0px 5px 0px #111;
  transition: transform .3s ease-in-out;
}
.active{
  transform: translate(-50%, -10px);
}

.error__icon {
  width: 20px;
  height: 20px;
  transform: translateY(-2px);
  margin-right: 8px;
}

.error__icon path {
  fill: #EF665B;
}

.error__title {
  font-weight: 500;
  font-size: 14px;
  color: #71192F;
}

.error__close {
  width: 20px;
  height: 20px;
  cursor: pointer;
  margin-left: auto;
}

.error__close path {
  fill: #71192F;
}</style>