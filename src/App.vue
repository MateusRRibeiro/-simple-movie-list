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
          placeholder="Ex: Interestelar, Jogos Mortais, O ultimo..."
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
          @editarFilmeLista="abrirModalEditarFilme"
        />
      </div>
    </div>

    <VModal :modalEstaAtivo="modalEstaAtivo" @fecharModal="fecharModal">
      <div class="conteudo-modal">
        <input
          class="input-filme"
          v-model="filmeEditar.nome"
          type="text"
        />

        <VButton @click="editarFilme" nome="Editar Filme"/>
      </div>
    </VModal>

    <VNotification
      :texto="notificacaoTexto"
      :iniciar='deveIniciarNotificacao'
      @desativa-notificacao="desativaNotificacao"
    />
  </div>
</template>

<script>
import axios from 'axios';
import GlobalStyle from './style/global.scss';

import VList from './components/VList.vue';
import VButton from './components/VButton.vue';
import VModal from './components/VModal.vue';
import VNotification from './components/VNotification.vue';

export default {
  name: 'App',
  data: () => ({
    nomeNovoFilme: '',
    listaDeFilmes: [],
    appFoiCarregado: false,
    filmeEditar: {
      nome: '',
    },
    modalEstaAtivo: false,
    notificacaoTexto: '',
    deveIniciarNotificacao: false,
  }),
  components: {
    VList,
    VButton,
    VModal,
    VNotification,
  },
  methods: {
    async removerItemLista(filmeRemovido) {
      this.listaDeFilmes = this.listaDeFilmes.filter((filme) => filme.id !== filmeRemovido.id);
      await this.removerFilme(filmeRemovido);
    },
    async adicionarItemLista(nomeFilme) {
      const nomeFilmeSemEspacos = nomeFilme.trim();

      if (nomeFilmeSemEspacos === '') {
        this.iniciarNotificacao('Informe o nome do filme!');
        return;
      }

      try {
        await axios.post('http://localhost:8000/api/filmes', { nome: nomeFilmeSemEspacos });
        await this.consultarFilmes();
        this.nomeNovoFilme = '';
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
    async editarFilme() {
      await this.alterarFilme(this.filmeEditar);
      this.fecharModal();
    },
    async abrirModalEditarFilme(filmeEditar) {
      this.filmeEditar = filmeEditar;
      this.modalEstaAtivo = true;
    },
    fecharModal() {
      this.modalEstaAtivo = false;
    },
    iniciarNotificacao(texto) {
      this.notificacaoTexto = texto;
      this.deveIniciarNotificacao = true;
    },
    desativaNotificacao() {
      this.notificacaoTexto = '';
      this.deveIniciarNotificacao = false;
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
    padding: 12px;
    flex-direction: column;
    align-items: center;
    margin-bottom: 32px;
  }

  .conteudo-modal{
    padding: 12px;
    display: flex;
    flex-direction: column;
    gap: 32px;
    align-items: center;

    &::v-deep .input-filme{
      display: flex;
      min-width: calc(100% - 24px);
      min-height: 40px;
      border-radius: 25px;
      padding: 8px 12px;
      border: none;
      font-size: 18px;
    }
  }

</style>
