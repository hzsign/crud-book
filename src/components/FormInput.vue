<script setup>
const props = defineProps({
  type: {
    type: String,
    default: "text",
  },
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
    <input
      :type="props.type"
      :value="props.modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      class="border-gray-300"
    />
  </div>
</template>
