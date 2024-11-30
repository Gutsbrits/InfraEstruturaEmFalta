<template>
  <div id="app">
    <!-- Barra de navegção -->
    <nav class="navbar"> 
      <div class="navbar-logo">
        <a href="#">InfraEstrutura em Falta</a>
      </div>
      <ul class="navbar-links">
        <li v-if="isLoggedIn">
          <!-- Botão para visualizar a página de reclamações -->
          <a href="#" @click.prevent="currentPage = 'Visualizar'">Visualizar Reclamações</a>
        </li>
        <li v-if="isLoggedIn">
          <!-- Botão para voltar para a página de regitro de reclamações  -->
          <a href="#" @click.prevent="currentPage = 'Reclamacoes'">Registrar Reclamação</a>
        </li>
        <li v-if="isLoggedIn">
          <!-- Botão de sair -->
          <a href="#" @click.prevent="logout">Sair</a>

          <!-- @click.prevent executa uma ação e evita o comportamento padrão de recarregar a página -->
        </li>

      </ul>
    </nav>
    <!-- Fim da barra de navegação -->
    <component
      :is="currentPage"
      @login-success="handleLoginSuccess"
      @go-to-cadastro="currentPage = 'Cadastro'"
      @go-to-login="currentPage = 'Login'"
    />
    <!-- Um componente dinâmico que renderiza o componente baseado no valor de currentPage. -->
  </div>
</template>

<script>
import Login from "./components/Login.vue";
import Cadastro from "./components/Cadastro.vue";
import Reclamacoes from "./components/Reclamacoes.vue";
import Visualizar from "./components/VisualizarReclamacoes.vue";

export default {
  data() {
    return {
      currentPage: "Login", // Controla qual página está ativa
      isLoggedIn: false, // Indica se o usúraio está logado
    };
  },
  components: {
    // Vai importar os componentes que serão usados no componente dinâmico.
    Login,
    Cadastro,
    Reclamacoes,
    Visualizar,
  },
  methods: {
    handleLoginSuccess() { // Método chamado quando usúario faz login.
      this.currentPage = "Reclamacoes";
      this.isLoggedIn = true; // Atualiza o isLoggedIn e muda para a página de registro de reclamações.
    },
    logout() { // Método para deslogar o usúario.
      localStorage.removeItem("loggedUser"); // Remove informações do LcalStorage
      this.isLoggedIn = false; 
      this.currentPage = "Login";
    },
  },
};
</script>

<style scoped>
/* Barra de Navegação */
.navbar {
  background-color: #333;
  padding: 15px 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: white;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  margin: 0 auto;
}

.navbar-logo a {
  color: #fff;
  font-size: 24px;
  font-weight: bold;
  text-decoration: none;
}

.navbar-links {
  display: flex;
  list-style: none;
}

.navbar-links li {
  margin-left: 20px;
}

.navbar-links a {
  color: #fff;
  font-size: 16px;
  text-decoration: none;
  transition: color 0.3s ease;
}

.navbar-links a:hover {
  color: #f1c40f;
}
</style>
