<template>
    <div class="notificacao"
        :class="{'notificacao--ativo': deveExibirNotificacao}"
        @click="desativarNotificacao"
    >
        {{texto}}
    </div>
</template>

<script>
export default {
  name: 'VButton',
  data: () => ({
    deveExibirNotificacao: false,
  }),
  props: {
    texto: {
      type: String,
      required: true,
    },
    iniciar: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    iniciarNofiticacao() {
      this.deveExibirNotificacao = true;
    },
    desativarNotificacao() {
      this.deveExibirNotificacao = false;
      this.$emit('desativa-notificacao');
    },
  },
  watch: {
    iniciar(novoValor) {
      if (novoValor === true) {
        this.iniciarNofiticacao();

        setTimeout(() => {
          this.desativarNotificacao();
        }, 3000);
      }
    },
  },
};
</script>

<style lang="scss">
  .notificacao{
    min-width: 100%;
    min-height: 0;
    max-height: 0;
    align-items: center;
    justify-content: center;
    position: fixed;
    bottom: -82px;
    background: #095e6e;
    color: white;
    cursor: pointer;
    left: 0;
    overflow:hidden;
    transition: all .5s ease;
    display: flex;
    z-index: 999;

    &--ativo{
        bottom: 0;
        min-height: 82px;
        max-height: 82px;
    }
  }
</style>
