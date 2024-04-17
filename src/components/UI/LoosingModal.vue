<template>
  <div class="modal">
    <div class="modal__window">
      <div class="modal__window__content">
        <h1>Вы проиграли</h1>
        <p>Вы продержались: {{ round }}</p>
      </div>

      <div class="modal__window__actions">
        <button @click="$emit('close')">Закрыть</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {};
  },
  emits: ["close"],
  props: {
    round: Number,
  },
};
</script>

<style scoped>
@import url("@/styles/components/UI/LoosingModal.css");
</style>
