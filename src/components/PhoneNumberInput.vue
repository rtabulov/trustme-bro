<script setup lang="ts">
import { computed, ref, watch } from "vue";
import svg from "country-list-with-dial-code-and-flag/dist/flag-svg";
import { nanoid } from "nanoid";
import { phone } from "phone";

const MAX_PHONE_LENGTH = 15;

interface Props {
  label?: string;
  errorMessage?: string;
  id?: string;
  modelValue: string;
}

const props = withDefaults(defineProps<Props>(), {
  label: "Номер телефона",
  errorMessage: "Неверный номер телефона",
});

const emit = defineEmits<{
  (e: "update:modelValue", val: string): void;
}>();

const touched = ref(false);
const valueProxy = computed({
  get: () => props.modelValue,
  set: (value) => emit("update:modelValue", value),
});

const inputId = computed(() => props.id || "id-" + nanoid());
const phoneInfo = computed(() => phone(valueProxy.value));
const countryFlagSvg = computed(() => svg[phoneInfo.value.countryIso2 || ""]);

watch(valueProxy, (newValue, oldValue) => {
  if (newValue.length > MAX_PHONE_LENGTH) {
    valueProxy.value = oldValue;
  }
});

watch(valueProxy, (newValue) => {
  valueProxy.value = "+" + newValue.replace(/[^0-9]/g, "");
});

// watchEffect(() => {
//   console.log({
//     phoneInfo: phoneInfo.value,
//     valueProxy: valueProxy.value,
//     countryFlagSvg: !!countryFlagSvg.value,
//   });
// });
</script>

<template>
  <div class="">
    <label class="block text-sm" :for="inputId">{{ label }}</label>
    <div class="inline-block relative">
      <input
        v-model="valueProxy"
        placeholder="+7___ ___ __ __"
        type="text"
        class="w-48 rounded-sm pr-10 text-blue-800 px-3 py-1 focus:outline-2 outline-1 outline"
        :class="{
          'outline-blue-600': !touched,
          'outline-red-600': touched && !phoneInfo.isValid,
          'outline-green-600': touched && phoneInfo.isValid,
        }"
        :id="inputId"
        @blur="touched = true"
      />
      <span class="block absolute right-2 -translate-y-1/2 top-1/2">
        <span class="w-6 block" v-html="countryFlagSvg"></span>
      </span>
    </div>
    <small v-if="touched && !phoneInfo.isValid" class="block text-red-600">
      {{ errorMessage }}
    </small>
  </div>
</template>
