<template>
    <li class="lista">
        <ul v-for="item in lista" :key="item.id" class="lista__linha">
          <input class="lista__input" type="checkbox" v-model="item.assistido" />
          {{item.nome}}
        </ul>
    </li>
</template>

<script>
export default {
  name: 'VList',
  props: {
    lista: {
      type: Array,
      default: () => [],
      required: true,
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
    }

    &__input{
      min-width: 24px;
      min-height: 24px;
    }
  }
</style>
