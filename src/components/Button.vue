<template>
  <button
    v-on:click="sort(buttonData, index)"
    class="btn "
    :class="{ isActive : buttonData.active, 'ma-10': $vuetify.breakpoint.xsAndDown }"
  >
    <v-icon
      small
      v-if="buttonData.asc"
    >arrow_upward</v-icon>
    <v-icon
      small
      v-if="!buttonData.asc"
    >arrow_downward</v-icon>
    {{ buttonData.text }}
  </button>
</template>

<style scoped>
.btn {
  display: inline-flex;
  align-items: center;
  color: #5e5e5f;
  border-radius: 100px;
  padding: 0.5rem 0.75rem;
  margin: 0 1rem;
  font-weight: 500;
  transition: all 100ms ease-in-out;
}

.btn:not(.isActive):hover {
  background-color: #efefef;
}

.isActive {
  color: #000;
  background-color: #fff;
  box-shadow: 2px 2px 12px #dfe4ff;
}

.v-icon {
  margin-right: 0.25rem;
}
</style>

<script>
export default {
  data: () => ({}),
  props: ["buttonData", "sort", "index"]
};
</script>


