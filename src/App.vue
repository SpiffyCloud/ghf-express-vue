<script setup>
import { ref, computed, onMounted } from "vue";
import { Preferences } from "@capacitor/preferences";

import BarcodeBlock from "@/components/BarcodeBlock.vue";
import KeyPadBlock from "@/components/KeypadBlock.vue";
import GraphicTopBottom from "@/components/graphics/GraphicTopBottom.vue";
import GraphicTopTop from "@/components/graphics/GraphicTopTop.vue";
import GraphicBottomBottom from "@/components/graphics/GraphicBottomBottom.vue";
import GraphicBottomTop from "@/components/graphics/GraphicBottomTop.vue";

const barcode = ref("");
const barcodeIsValid = computed(() => {
  return barcode.value.length === 6 && /^\d+$/.test(barcode.value);
});

onMounted(async () => {
  loadBarcode();
});

async function loadBarcode() {
  const { value } = await Preferences.get({ key: "barcode" });
  if (value) {
    barcode.value = value;
  }
}

async function saveBarcode(value) {
  await Preferences.set({ key: "barcode", value });
  barcode.value = value;
}

async function deleteBarcode() {
  await Preferences.remove({ key: "barcode" });
  barcode.value = "";
}
</script>

<template>
  <header>
    <h1>GHF Express</h1>
  </header>

  <Transition :name="barcodeIsValid ? '' : 'fade'" mode="out-in">
    <BarcodeBlock v-if="barcodeIsValid" :barcode="barcode" @delete="deleteBarcode" />
    <KeyPadBlock v-else @saveBarcode="saveBarcode" />
  </Transition>

  <footer>
    <a href="https://github.com/SpiffyCloud/ghf-express-vue" target="_blank">
      GHF Express v1.0.0 | A SpiffyCloud Project
    </a>
  </footer>

  <div class="theme">
    <GraphicTopBottom />
    <GraphicTopTop />
    <GraphicBottomBottom />
    <GraphicBottomTop />
  </div>
</template>

<style scoped>
h1 {
  margin: 0;
  padding-top: 4rem;
}

a {
  display: block;
  color: var(--color-button-press);
  width: 75%;
  font-size: 0.75rem;
  text-decoration: none;
  margin: 3rem auto;
}

.theme {
  position: absolute;
  height: 100%;
  width: 100%;
  z-index: -1;
}

.theme svg {
  position: absolute;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease-out;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
