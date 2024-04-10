<template>
  <div class="p-5">
    <Comps @on-select-comp="handleSelectComp" name="red"/>
    <div class="mt-10 flex flex-col">
      <div style="box-shadow: 0 8px 30px rgb(0,0,0,0.12);" class="grid grid-cols-2 divide-x w-full">
        <div><FontChecking :default-file="'GSPione-Regular_20211006.otf'" :name="'font-left'" :comp="selectedComp"/></div>
        <div><FontChecking :default-file="'Montserrat-Medium.otf'" :name="'font-right'" :comp="selectedComp"/></div>
      </div>
      <p class="text-sm text-pretty text-gray-500 text-end mt-5 italic w-2/3 self-end h-20"><span class="text-red-500">*</span>
        <template v-if="selectedComp === CompNames.Button">
          Note: When we look at the buttons, we can see that in most fonts,
          the text and icon are always centered evenly.
          However, the text will be pushed up and out of the center if you choose the GS Pione font.
        </template>
        <template v-else>
          Note: When we look at highlighted text (red text on a black background), we can see that in most fonts,
          the text is always in the middle of the highlighting box.
          However, the text will be pushed up and out of the center of the box if you choose the GS Pione font.
        </template>
      </p>
    </div>
  </div>
</template>
<script setup lang="ts">
import FontChecking from "./components/FontChecking.vue";
import Comps from "./components/Comps.vue";
import {compNameItems, CompNames} from "./constants.ts";
import {ref} from "vue";

const selectedComp = ref(compNameItems[0]);

const handleSelectComp = (compName: CompNames) => {
  selectedComp.value = compName;
};
</script>

<style scoped>
</style>
