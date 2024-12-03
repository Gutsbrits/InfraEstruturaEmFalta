<template>
  <div class="cadastro">
    <h2>Cadastro</h2>
    <form @submit.prevent="registrar">
      <div class="form-group">
        <label for="nome">Nome:</label>
        <input type="text" id="nome" v-model="nome" required />
      </div>
      <div class="form-group">
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="email" required />
      </div>
      <div class="form-group">
        <label for="senha">Senha:</label>
        <input type="password" id="senha" v-model="senha" required />
      </div>
      <button type="submit" class="submit-btn">Cadastrar</button>
    </form>
    <p class="link-login">Já tem uma conta? <a @click="$emit('go-to-login')">Faça login</a></p>
  </div>
</template>

<script>

import Localbase from 'localbase'

// let db = new Localbase('ief')

export default {
  data() {
    return {
      nome: '',
      email: '',
      senha: '',
      db: null
    };
  },
  mounted(){
   this.db =  new Localbase('ief')
  },
  methods: {
    registrar() {
      if (this.nome && this.email && this.senha) {
        const usuario = { nome: this.nome, email: this.email, senha: this.senha };
        // Salvar usuário no localStorage ou em uma base de dados
        this.db.collection('usuarios').add(
          usuario
        )
        localStorage.setItem("usuario", JSON.stringify(usuario));
        db.collection('users').get().then(users => {
  console.log(users)
})
        alert("Cadastro realizado com sucesso!");
        this.$emit("go-to-login"); // Navega para a tela de login após o cadastro
      } else {
        alert("Preencha todos os campos!");
      }
    },
  },
};
</script>

<style scoped>
.cadastro {
  max-width: 500px;
  margin: 40px auto;
  padding: 30px;
  background: #333;
  border-radius: 12px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
  color: #fff;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h2 {
  text-align: center;
  font-size: 26px;
  color: #f39c12;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

label {
  font-size: 14px;
  color: #bbb;
}

input {
  width: 100%;
  padding: 12px;
  margin-top: 5px;
  background: #444;
  border: 1px solid #666;
  border-radius: 5px;
  color: #fff;
  font-size: 16px;
}

input:focus {
  outline: none;
  border-color: #f39c12;
}

button.submit-btn {
  width: 100%;
  padding: 12px;
  background: #f39c12;
  color: #fff;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 20px;
}

button.submit-btn:hover {
  background: #e67e22;
}

.link-login {
  text-align: center;
  margin-top: 20px;
  font-size: 14px;
  color: #bbb;
}

.link-login a {
  color: #f39c12;
  text-decoration: none;
}

.link-login a:hover {
  text-decoration: underline;
}
</style>
