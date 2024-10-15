<script setup>
import { ref, computed, onMounted } from "vue";
import JsBarcode from "jsbarcode";

const props = defineProps({
  barcode: { type: String, required: true }
});
const emit = defineEmits(["delete"]);

const barcodeHeight = computed(() => {
  if (window.innerHeight >= 896) {
    return 150;
  }
  return 100;
});

onMounted(async () => {
  JsBarcode(".barcode", props.barcode, {
    format: "CODE39",
    width: 2,
    height: barcodeHeight.value,
    displayValue: true,
    fontOptions: "",
    font: "sans-serif",
    textAlign: "center",
    textPosition: "bottom",
    textMargin: 5,
    fontSize: 20,
    background: "var(--color-secondary)",
    lineColor: "var(--color-primary)",
    margin: 10,
  });
});
</script>

<template>
  <main>
    <p>Scan the barcode to check into the club</p>
    <div class="barcode-container">
      <div class="barcode-sticker">
        <svg class="barcode"></svg>
      </div>
    </div>
    <button @click="emit('delete')" class="button">Delete</button>
  </main>
</template>

<style scoped>
.barcode-container {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}

.barcode-sticker {
  background-color: var(--color-secondary);
  margin: 0 auto;
  border-radius: 0.5rem;
  padding: 0.5rem;
  width: calc(100% - 4rem);
  max-width: 20rem;
  box-shadow: 0px 16px 16px rgba(0, 0, 0, 0.4);
}

.barcode-sticker svg {
  width: 100%;
  height: 100%;
}

.button {
  color: var(--color-danger);
  font-family: inherit;
  background-color: transparent;
  border: none;
  font-size: 1rem;
  border-radius: 0.5rem;
  padding: 1rem 2rem;
  margin: 0 auto;
  margin-bottom: 7rem;
  transition: background ease-out 100ms;
}

.button:active {
  background-color: var(--color-button-danger);
}

p {
  padding-top: 4rem;
}

@media screen and (min-height: 896px) {
  .button {
    font-size: 1.25rem;
  }

  p {
    padding-top: 6rem;
    font-size: 1.25rem;
  }
}
</style>
