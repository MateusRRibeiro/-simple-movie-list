<template>
    <li class="lista">
        <ul v-for="item in lista" :key="item.id" class="lista__linha">
          <input
            class="lista__input"
            type="checkbox"
            v-model="item.assistido"
          />
          {{item.nome}}
          <IconX @click="removerItem(item.id)" class="icone-remover"/>
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
  }

  .icone-remover{
    fill: #e50000;
  }
</style>
