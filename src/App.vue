<template>
<div>
  <header>
    <p>Minha Lista de Filmes</p>
  </header>

  <div class="container-formulario">
    <input
      class="input-filme"
      v-model="nomeNovoFilme"
      type="text"
      placeholder="Ex: Interistelar, Jogos Mortais, O ultimo..."
    />
  </div>

  <div class="container-botoes">
    <VButton nome="Adicionar Filme"/>
  </div>

  <div class="container-lista">
    <VList :lista="listaDeFilmes"/>
  </div>
</div>

</template>

<script>
import GlobalStyle from './style/global.scss';

import VList from './components/VList.vue';
import VButton from './components/VButton.vue';

export default {
  name: 'App',
  data: () => ({
    nomeNovoFilme: '',
    listaDeFilmes: [
      { id: 1, nome: 'Quem quer ser um milhonário', assistido: false },
      { id: 2, nome: 'O hoje já é amanhã', assistido: true },
    ],
  }),
  components: {
    VList,
    VButton,
  },
};
</script>

<style lang="scss" scoped>
  .container-formulario{
    display: flex;
    justify-content: center;
    margin-bottom: 32px;

    .input-filme{
      min-width: 358px;
      min-height: 40px;
      border-radius: 25px;
      padding: 8px 12px;
      border: none;
      font-size: 18px;
    }
  }

  .container-botoes{
    display: flex;
    gap: 12px;
    justify-content: center;
    margin-bottom: 32px;
  }

  .container-lista{
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 32px;
  }

</style>
