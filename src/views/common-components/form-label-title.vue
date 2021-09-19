<template>
  <div class="label-title">
    <slot name="title">title</slot>
  </div>
</template>

<script>

export default {
  name: 'FormLabelTitle'
}
</script>

<style lang="scss" scoped>
.label-title {
  font-size: 18px;
  color: #282828;
  background: #fff;
  padding: 0 10px;
  font-weight: bold;
  margin-bottom:18px;
}
</style>
