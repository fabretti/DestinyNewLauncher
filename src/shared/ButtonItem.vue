<template>
  <button
    class="ButtonItem"
    :class="[variant && `variant--${variant}`, size && `size--${size}`, color && `color--${color}`]"
    type="button"
    :disabled="disabled"
  >
    <slot />
  </button>
</template>
<script setup lang="ts">
defineProps({
  text: {
    type: String,
    default: 'base',
  },
  disabled: {
    type: Boolean,
    default: false,
  },
})
</script>

<style lang="scss">
.ButtonItem {
  height: 55px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  border: 1px solid rgba(103, 201, 247, 0.3);
  background: rgba(35, 67, 82, 0.12);
  box-shadow:
    0px 0px 15px 0px rgba(103, 201, 247, 0.18),
    0px 0px 40px 0px rgba(43, 109, 141, 0.72) inset;
  transition: box-shadow 0.3s ease;
  color: rgba(189, 234, 255, 1);
  font-weight: 400;
  font-size: 18px;
  line-height: 100%;
  letter-spacing: 2px;

  gap: 8px;
  cursor: pointer;
  white-space: nowrap;
  backdrop-filter: blur(10px);

  &:disabled {
    opacity: 0.5;
  }

  &:hover {
    box-shadow: 0px 0px 40px 0px rgba(43, 109, 141, 0.72) inset;
  }
}
</style>
