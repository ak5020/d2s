<template>
  <div class="dd-base" v-bind="$attrs">
    <label v-if="label" class="dd-block dd-text-sm dd-font-medium dd-text-gray-700 dd-mb-1">{{ label }} <span
        v-if="isRequired" class="dd-text-red-500 ">*</span> </label>
    <div class="dd-relative  dd-rounded-md ">
      <div v-if="prefix"
        class="!dd-pointer-events-none !dd-absolute !dd-inset-y-0 !dd-left-0 !dd-flex !dd-items-center !dd-pl-3 !dd-pr-10">
        <svgIcon class="dd-text-gray-400" :icon="icon" :size="btnIconSize" />
      </div>
      <input
      :disabled="disabled"
        :class="[inputSize, suffix ? 'dd-pr-10' : 'dd-pr-2', prefix ? 'dd-pl-10' : 'dd-pl-2', hasError ? '!dd-border-red-600' : '!dd-border-gray-300', errorMessage ? 'dd-mb-1' : '',disabled ? '!dd-text-gray-500 dd-ring-gray-200 dd-bg-gray-50 dd-cursor-not-allowed dd-select-none' : 'focus:dd-ring-teal-600 focus:!dd-border-teal-600 dd-text-gray-700']"
        v-model="inputModelValue" :type="inputType"
        class="dd-border-solid !dd-block !dd-w-full !dd-rounded-md    dd-focus:!dd-ring-teal-600 sm:!dd-text-sm focus:ring-2 focus:dd-ring-inset  dd-shadow-sm"
        :placeholder="placeholder" />
      <!-- $slots.suffix -->
      <div @click="suffixIconClick" v-if="suffix && suffixIcon"
        class="dd-cursor-pointer !dd-absolute !dd-inset-y-0 !dd-right-0 !dd-flex !dd-items-center !dd-pl-3 !dd-pr-3">
        <svgIcon  class="dd-text-gray-400" :icon="suffixIcon" :size="btnIconSize" />
        <!-- <slot name="suffix">
        </slot> -->
      </div>
      <span v-if="errorMessage" class="dd-text-xs dd-text-red-500 dd-capitalize dd-pt-px">{{ errorMessage }}</span>
    </div>
  </div>
</template>

<script setup>
import svgIcon from "../svgIcon/index.vue"
import { ref, computed, watch } from "vue"
const emits = defineEmits( ['update:modelValue', "change", "suffixIconClick"] )
const props = defineProps( {
  label: {
    type: String,
    default: "",
  },
  prefix: {
    type: Boolean,
    default: false,
  },
  suffix: {
    type: Boolean,
    default: false,
  },
  disabled:{
    type: Boolean,
    default: false
  },
  icon: {
    type: String,
    default: null,
  },
  errorMessage: {
    type: String,
    default: "",
  },
  isRequired: {
    type: Boolean,
    default: false,
  },
  placeholder: {
    type: String,
    default: "",
  },
  type: {
    type: String,
    default: "text",
  },
  modelValue: {
    type: [String, Number],
    default: null,
  },
  size: {
    type: String,
    validator: function ( value ) {
      // The value must match one of these strings
      return (
        ["xs", "sm", "base", "lg", "xl"].indexOf( value ) !== -1
      )
    },
    default: "base",
  },
} )
const inputType = ref( 'text' )
const inputSize = computed( () => {
  return {
    "dd-h-6 !dd-text-xs": props.size === "xs",
    "dd-h-7  ": props.size === "sm",
    "dd-h-8 ": props.size === "base",
    "dd-h-9 ": props.size === "lg",
    "dd-h-10 ": props.size === "xl",
  }
} )
const inputModelValue = computed( {
  get () {
    return props.modelValue
  },
  set ( val ) {
    emits( "update:modelValue", val )
    emits( "change", val )
  }
} )
const hasError = computed( () => {
  if ( props.errorMessage ) {
    return true
  } else {
    return false
  }
} )
const btnIconSize = computed( () => {
  if ( props.size == 'xs' ) {
    return '10'
  } else if ( props.size == 'sm' ) {
    return '12'
  } else {
    return '16'
  }
} )
const suffixIcon = computed( () => {
  if ( props.icon) {
    return props.icon
  } else if (props.type == 'password' && inputType.value == 'text') {
    return 'Eyeoff'
  }  else{
    return 'Eye'
  }
} )
const suffixIconClick = () => {
  if ( props.type == 'password' ) {
    inputType.value = inputType.value == "password" ? 'text' : 'password'
  }
  emits( 'suffixIconClick', true )
}
watch( () => props.type, ( newVal ) => {
  inputType.value = newVal
},{immediate: true} )
</script>

<style lang="css" scoped>
input {
  padding-top: 0.5em;
  padding-bottom: 0.5em;
}</style>
