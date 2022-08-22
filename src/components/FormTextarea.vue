<script setup>
const props = defineProps({
  modelValue: {
    type: String,
  },
});

defineEmits(["update:modelValue"]);
</script>

<template>
  <div>
    <label class="block">
      <slot></slot>
    </label>
    <textarea
      :value="props.modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      class="border-gray-300"
    ></textarea>
  </div>
</template>
