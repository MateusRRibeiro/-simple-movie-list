<template>
    <li class="lista">
        <p v-if="lista.length === 0">Adicione um filme a sua lista!</p>
        <ul v-else v-for="item in lista" :key="item.id" class="lista__linha">
          <input
            class="lista__input"
            type="checkbox"
            v-model="item.assistido"
            @change="atualizarValorLista(item)"
          />
          <p
            class="lista__nome"
            :class="{'lista__nome--ativo': item.assistido}"
          >{{item.nome}}</p>
          <IconX @click="removerItem(item)" class="icone-remover"/>
        </ul>
    </li>
</template>

<script>
import IconX from '../assets/IconX.svg';

export default {
  name: 'VList',
  props: {
    lista: {
      type: Array,
      default: () => [],
      required: true,
    },
  },
  components: {
    IconX,
  },
  methods: {
    removerItem(idItem) {
      this.$emit('remover-item-evento', idItem);
    },
    atualizarValorLista(item) {
      this.$emit('atualizar-valor-lista', item);
    },
  },
};
</script>

<style lang="scss">
  .lista{
    list-style-type: none;
    display: flex;
    flex-direction: column;
    gap: 16px;

    &__linha{
      display: flex;
      gap: 12px;
      align-content: center;
    }

    &__input{
      min-width: 24px;
      min-height: 24px;
    }

    &__nome--ativo{
      text-decoration: line-through;
    }
  }

  .icone-remover{
    fill: #e50000;
  }
</style>
