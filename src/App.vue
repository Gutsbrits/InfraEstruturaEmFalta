<template>
  <div id="app">
    <nav class="navbar">
      <div class="navbar-logo">
        <a href="#">InfraEstrutura em Falta</a>
      </div>
      <ul class="navbar-links">
        <li v-if="isLoggedIn">
          <a href="#" @click.prevent="currentPage = 'Visualizar'">Visualizar Reclamações</a>
        </li>
        <li v-if="isLoggedIn">
          <a href="#" @click.prevent="currentPage = 'Reclamacoes'">Registrar Reclamação</a>
        </li>
        <li v-if="isLoggedIn">
          <a href="#" @click.prevent="logout">Sair</a>
        </li>
      </ul>
    </nav>
    <component
      :is="currentPage"
      @login-success="handleLoginSuccess"
      @go-to-cadastro="currentPage = 'Cadastro'"
      @go-to-login="currentPage = 'Login'"
    />
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
      currentPage: "Login", // Página inicial
      isLoggedIn: false, // Controle de login
    };
  },
  components: {
    Login,
    Cadastro,
    Reclamacoes,
    Visualizar,
  },
  methods: {
    handleLoginSuccess() {
      this.currentPage = "Reclamacoes";
      this.isLoggedIn = true; // Define como logado
    },
    logout() {
      localStorage.removeItem("loggedUser"); // Remove usuário logado
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
