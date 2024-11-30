<template>
  <div class="login">
    <!-- Famoso titulo -->
    <h2>Login</h2>
    <form @submit.prevent="login">
      <div class="form-group">
        <label for="username">Email:</label>
        <input v-model="username" type="email" id="username" required />
      </div>
      <div class="form-group">
        <label for="password">Senha:</label>
        <input v-model="password" type="password" id="password" required />
      </div>
      <button type="submit">Entrar</button>
    </form>
    <p v-if="error" class="error">{{ error }}</p>
    <p>
      Não tem uma conta?
      <button @click="$emit('go-to-cadastro')" class="link-btn">Cadastre-se aqui</button>
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: "",
      password: "",
      error: "",
    };
  },
  methods: {
    login() {
      const usuarios = JSON.parse(localStorage.getItem("usuarios")) || [];
      const usuarioValido = usuarios.find(
        (user) => user.email === this.username && user.senha === this.password
      );

      if (usuarioValido) {
        localStorage.setItem("loggedUser", this.username);
        this.$emit("login-success");
      } else {
        this.error = "Email ou senha inválidos!";
      }
    },
  },
};
</script>

<style scoped>
.login {
  max-width: 400px;
  margin: 40px auto;
  padding: 20px;
  background: #2d2d2d;
  color: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

h2 {
  font-size: 24px;
  text-align: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  font-size: 14px;
}

input[type="email"],
input[type="password"] {
  width: 378px;
  padding: 10px;
  font-size: 19px;
  margin-top: 5px;
  border-radius: 100px;
  border: 1px solid #ccc;
  background-color: #444;
  color: #fff;
}

button {
  width: 100%;
  padding: 12px;
  background: #f39c12;
  color: #fff;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background: #e67e22;
}

.error {
  color: #e74c3c;
  margin-top: 10px;
  text-align: center;
  font-size: 14px;
}

.link-btn {
  background: none;
  color: #f39c12;
  border: none;
  font-size: 14px;
  cursor: pointer;
}

.link-btn:hover {
  text-decoration: underline;
}
</style>
