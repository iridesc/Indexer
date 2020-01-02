<template>
  <div class="susp">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "FootBar"
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.susp {
  position: fixed;
  bottom: 1%;
  width: 100%;
}
</style>
