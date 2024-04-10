<template>
  <div class="p-5 flex flex-col" style="min-height: 70vh; width: 100%">
    <div class="flex-1">
      <component :is="comp" :style="{ fontFamily: name }" :name="name" />
    </div>
    <div class="text-md flex justify-end items-center gap-0.5 h-5 mt-5">
      <template v-if="uploadedFile">
        <svg id="Layer_1" fill="#000000" height="14px" version="1.1" viewBox="0 0 507.2 507.2"
             width="14px" xml:space="preserve" xmlns="http://www.w3.org/2000/svg"
             xmlns:xlink="http://www.w3.org/1999/xlink">
              <g id="SVGRepo_bgCarrier" stroke-width="0"/>
          <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"/>

          <g id="SVGRepo_iconCarrier"> <circle cx="253.6" cy="253.6" r="253.6" style="fill:#32BA7C;"/>
            <path d="M188.8,368l130.4,130.4c108-28.8,188-127.2,188-244.8c0-2.4,0-4.8,0-7.2L404.8,152L188.8,368z"
                  style="fill:#0AA06E;"/>
            <g> <path d="M260,310.4c11.2,11.2,11.2,30.4,0,41.6l-23.2,23.2c-11.2,11.2-30.4,11.2-41.6,0L93.6,272.8 c-11.2-11.2-11.2-30.4,0-41.6l23.2-23.2c11.2-11.2,30.4-11.2,41.6,0L260,310.4z"
                      style="fill:#FFFFFF;"/>
              <path d="M348.8,133.6c11.2-11.2,30.4-11.2,41.6,0l23.2,23.2c11.2,11.2,11.2,30.4,0,41.6l-176,175.2 c-11.2,11.2-30.4,11.2-41.6,0l-23.2-23.2c-11.2-11.2-11.2-30.4,0-41.6L348.8,133.6z"
                    style="fill:#FFFFFF;"/> </g> </g>
              </svg>
        {{ uploadedFile }}
      </template>
    </div>
    <div :id="inputId"  class="flex items-center justify-center w-full mt-2">
      <label class="flex flex-col items-center justify-center w-full h-30 border-2 border-gray-300 border-dashed rounded-lg cursor-pointer bg-gray-50 dark:hover:bg-bray-800 dark:bg-gray-700 hover:bg-gray-100 dark:border-gray-600 dark:hover:border-gray-500 dark:hover:bg-gray-600"
             :for="name">
        <div class="flex flex-col items-center justify-center pt-5 pb-6">
          <svg aria-hidden="true" class="w-8 h-8 mb-4 text-gray-500 dark:text-gray-400"
               fill="none" viewBox="0 0 20 16" xmlns="http://www.w3.org/2000/svg">
            <path d="M13 13h3a3 3 0 0 0 0-6h-.025A5.56 5.56 0 0 0 16 6.5 5.5 5.5 0 0 0 5.207 5.021C5.137 5.017 5.071 5 5 5a4 4 0 0 0 0 8h2.167M10 15V6m0 0L8 8m2-2 2 2" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
                  stroke-width="2"/>
          </svg>
          <p class="mb-2 text-sm text-gray-500 dark:text-gray-400"><span class="font-semibold">Click to upload a font file</span>
          </p>
          <p class="text-xs text-gray-500 dark:text-gray-400">OTF, TTF files</p>
        </div>
        <input :id="name" accept=".otf, .ttf" class="hidden" type="file" @change="previewFont"/>
      </label>
    </div>
  </div>
</template>
<script lang="ts" setup>
import {computed, defineProps, ref} from 'vue'
import {CompNames} from "../constants.ts";
import CardComp from "./CardComp.vue";
import BreadcrumbComp from "./BreadcrumbComp.vue";
import ButtonComp from "./ButtonComp.vue";

const props = defineProps({
  name: {
    type: String,
    required: true
  },
  comp: {
    type: String,
    required: true
  },
  defaultFile: {
    type: String,
    required: true
  }
})
const uploadedFile = ref(props.defaultFile);
const components = {
  [CompNames.Breadcrumb]: BreadcrumbComp,
  [CompNames.Card]: CardComp,
  [CompNames.Button]: ButtonComp,
};
const inputId = `${props.name}-input`;
const comp = computed(() => components[props.comp as CompNames]);
const previewFont = (event: Event) => {
  const files = (event.target as HTMLInputElement).files;
  if (!files) return;
  for (let i = 0; i < files.length; i++) {
    const file = files[i];
    uploadedFile.value = file.name;
    const reader = new FileReader();

    reader.onload = (e: ProgressEvent<FileReader>) => {
      const font = new FontFace(props.name, e.target?.result as ArrayBuffer);
      font.load().then((loadedFont) => {
        document.fonts.add(loadedFont);
      });
    };
    reader.readAsArrayBuffer(file);
  }
};
</script>

<style scoped>
</style>
