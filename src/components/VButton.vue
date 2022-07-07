<template>
    <div class="botao">
        {{nome}}
    </div>
</template>

<script>
export default {
  name: 'VButton',
  props: {
    nome: {
      type: String,
      required: true,
    },
  },
};
</script>

<style lang="scss">
  .botao{
    border-radius: 25px;
    width: fit-content;
    padding: 8px 12px;
    background-color: #094854;
    cursor: pointer;
  }
</style>
