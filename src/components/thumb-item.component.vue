<template>
    <div class="card">
      <img v-bind:src="thumb.url" class="img-fluid">
    </div>
</template>

<script>
export default {
  name: "thumb",
  props: ["thumb"]
};
</script>

