<template>
  <div class="wrapper">
    <input
      :id="id"
      type="checkbox"
      :checked="isChecked"
      :value="id"
      @click="onClick(id)"
    >
    <label :for="id">{{ label }}</label>
  </div>
</template>

<script>
export default {
  components: {},
  props: {
    label: {
      type: String,
      default: ""
    },
    onClick: {
      type: Function,
      default: () => {}
    },
    id: {
      type: String,
      required: true
    },
    isChecked: {
      type: Boolean,
      default: false
    }
  }
};
</script>

<style scoped lang="scss">
.wrapper {
  display: flex;

  label {
    padding-left: 5px;
  }
}
</style>
