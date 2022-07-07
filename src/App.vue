<template>
  <div>
    <header>
      <p>Minha Lista de Filmes</p>
    </header>

    <div v-if="appFoiCarregado">
      <div class="container-formulario">
        <input
          class="input-filme"
          v-model="nomeNovoFilme"
          type="text"
          placeholder="Ex: Interistelar, Jogos Mortais, O ultimo..."
        />
      </div>

      <div class="container-botoes">
        <VButton @click="adicionarItemLista(nomeNovoFilme)" nome="Adicionar Filme"/>
      </div>

      <div class="container-lista">
        <VList
          :lista="listaDeFilmes"
          @removerItemEvento="removerItemLista"
          @atualizarValorLista="alterarFilme"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import GlobalStyle from './style/global.scss';

import VList from './components/VList.vue';
import VButton from './components/VButton.vue';

export default {
  name: 'App',
  data: () => ({
    nomeNovoFilme: '',
    listaDeFilmes: [],
    appFoiCarregado: false,
  }),
  components: {
    VList,
    VButton,
  },
  methods: {
    async removerItemLista(filmeRemovido) {
      this.listaDeFilmes = this.listaDeFilmes.filter((filme) => filme.id !== filmeRemovido.id);
      await this.removerFilme(filmeRemovido);
    },
    async adicionarItemLista(nomeFilme) {
      try {
        await axios.post('http://localhost:8000/api/filmes', { nome: nomeFilme });
        await this.consultarFilmes();
      } catch (erro) {
        console.error(erro);
      }
    },
    async consultarFilmes() {
      const response = await axios.get('http://localhost:8000/api/filmes');
      this.listaDeFilmes = response.data;
    },
    async alterarFilme(filmeAlterado) {
      const filmeLista = this.listaDeFilmes.find((filme) => filme.id === filmeAlterado.id);

      if (filmeLista === undefined) {
        return;
      }

      try {
        await axios.patch(`http://localhost:8000/api/filmes/${filmeAlterado.id}`, { ...filmeAlterado });
      } catch (erro) {
        console.error(erro);
      }
    },
    async removerFilme(filmeRemovido) {
      try {
        await axios.delete(`http://localhost:8000/api/filmes/${filmeRemovido.id}`);
      } catch (erro) {
        console.error(erro);
      }
    },
  },
  async created() {
    await this.consultarFilmes();

    this.appFoiCarregado = true;
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
