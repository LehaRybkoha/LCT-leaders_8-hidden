<script setup>
defineProps({
  value: {
    type: String,
  },
  type: {
    type: String,
    default: () => 'text',
  },
  isErrored: Boolean,
})

const emit = defineEmits(['update:modelValue'])

const updateInput = (ev) => {
  emit('update:modelValue', ev.target.value)
}
</script>

<template>
  <input
    :type="type"
    :class="{ error: isErrored }"
    class="input"
    @input="updateInput"
    :value="value"
  />
</template>

<style lang="scss" scoped>
.input {
  border: 1px solid $border-color;
  padding: 10px 20px 10px 10px;
  border-radius: 5px;
  transition: border 0.3s ease;
  color: #000000;
  &:hover {
    border: 1px solid $accent-purple;
  }
  &.error {
    border: 1px solid red;
  }
}
</style>
