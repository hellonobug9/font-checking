<template>
  <div class="p-5">
    <Comps id="comps" @on-select-comp="handleSelectComp" name="red"/>
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
    <VOnboardingWrapper ref="wrapper" :steps="steps" />
  </div>
</template>
<script setup lang="ts">
import FontChecking from "./components/FontChecking.vue";
import Comps from "./components/Comps.vue";
import {compNameItems, CompNames} from "./constants.ts";
import {onMounted, ref} from "vue";
import { VOnboardingWrapper, useVOnboarding } from 'v-onboarding'

const selectedComp = ref(compNameItems[0]);
const handleSelectComp = (compName: CompNames) => {
  selectedComp.value = compName;
};
const wrapper = ref(null)
const { start } = useVOnboarding(wrapper)
const steps = [
  { attachTo: { element: '#font-left-btn' }, content: {
      title: "GS Pione font",
      description: "These buttons are designed with the GS Pione font. The text and icon are NOT centered evenly."
    } },
  { attachTo: { element: '#font-right-btn' }, content: {
      title: "Montserrat font",
      description: "Meanwhile these buttons are designed with the Montserrat font. The text and icon are centered evenly."
    } },
  { attachTo: { element: '#font-right-input' }, content: {
      title: "Upload font here...",
      description: "You can upload a font file here to see how it looks in the design."
    } },
  { attachTo: { element: '#comps' }, content: {
      title: "Demos font upload...",
      description: "You can see more demos by selecting different components in here."
    } }
]
onMounted(() => {
  setTimeout(() => {
    let isFirstLoadInSession = sessionStorage.getItem("isFirstLoadInSession");
    if(!isFirstLoadInSession) {
      start();
      sessionStorage.setItem("isFirstLoadInSession", "true")
    }
  }, 250);
})

</script>

<style scoped>
</style>
