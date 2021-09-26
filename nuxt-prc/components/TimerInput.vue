<!--
  数字以外は返さないようにしているため、値は整数のみとなる
-->
<template>
  <input
    type="number"
    class="no-spin"
    placeholder="3000 -> 30:00"
    :value="value"
    @input="$emit('input', $event.target.value.replace(/[^0-9]/g, ''))"
  >
</template>

<script>
export default {
  props: ['value']
}
</script>

<style lang="scss" scoped>
input {
  display: block;
  visibility: hidden;
  opacity: 0;
  position: absolute;
  left: 10px;
  bottom: 8px;
  width: 7.4em;
  height: 1.5em;
  padding: 0 0.6em;
  background-color: #aaa;
  border-radius: 4px;
  border: 1px solid #ddd;
  transition: all 0.2s linear;

  &:disabled {
    opacity: 0.4 !important;
  }
}

.no-spin::-webkit-inner-spin-button,
.no-spin::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
    -moz-appearance:textfield;
}
</style>