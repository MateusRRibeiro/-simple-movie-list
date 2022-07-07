<template>
    <div v-if="modalEstaAtivo" class="plano-de-fundo"></div>
    <div v-if="modalEstaAtivo" class="modal">
      <IconX @click="fecharModal" class="modal__icone"/>
      <slot></slot>
    </div>
</template>

<script>
import IconX from '../assets/IconX.svg';

export default {
  name: 'VModal',
  props: {
    modalEstaAtivo: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    IconX,
  },
  methods: {
    fecharModal() {
      this.$emit('fechar-modal');
    },
  },
};
</script>

<style lang="scss" scoped>
  .plano-de-fundo{
    position: fixed;
    top: 0;
    min-width: 100vw;
    min-height: 100vh;
    background-color: rgb(0 0 0 / 40%);
  }

  .modal{
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 64px;
    z-index: 2;
    min-width: 100vw;
    min-height: 100vh;
    background-color: #0f0d26;

    &__icone{
      margin: 0 12px 0 auto;
      fill: white;
      cursor: pointer;
    }
  }

</style>
