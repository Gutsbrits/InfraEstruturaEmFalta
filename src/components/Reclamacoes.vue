<template>
  <div class="reclamacoes">
    <h2>Registrar Reclamação</h2>
    <form @submit.prevent="enviarReclamacao">
      <div class="form-group">
        <label for="descricao">Descrição do Problema:</label>
        <textarea v-model="descricao" id="descricao" rows="4" required></textarea>
      </div>
      <div class="form-group">
        <p>Selecione o(s) tipo(s) de problema:</p>
        <div v-for="(tipo, index) in tiposDeProblema" :key="index">
          <label>
            <input
              type="checkbox"
              :value="tipo"
              v-model="problemasSelecionados"
            />
            {{ tipo }}
          </label>
        </div>
      </div>
      <button type="submit">Enviar Reclamação</button>
    </form>
    <p v-if="sucesso" class="success">Reclamação enviada com sucesso!</p>
  </div>
</template>

<script>
import Localbase from 'localbase'

export default {
  data() {
    return {
      descricao: "",
      tiposDeProblema: [
        "Buraco na rua",
        "Iluminação pública apagada",
        "Falta de coleta de lixo",
        "Esgoto a céu aberto",
        "Falta de sinalização",
        "Outros",
      ],
      problemasSelecionados: [],
      sucesso: false,
      db: null
    };
  },
  mounted(){
   this.db =  new Localbase('ief')

  },
  methods: {
    enviarReclamacao() {
      if (this.problemasSelecionados.length === 0) {
        alert("Por favor, selecione pelo menos um tipo de problema.");
        return;
      }

      const novaReclamacao = {
        descricao: this.descricao,
        problemas: this.problemasSelecionados[0],
        data: new Date().toLocaleString(),
      };


      this.db.collection('reclamacoes').add(
        novaReclamacao
      )
      const reclamacoes = JSON.parse(localStorage.getItem("reclamacoes")) || [];
      
      reclamacoes.push(novaReclamacao);
      
      localStorage.setItem("reclamacoes", JSON.stringify(reclamacoes));

      this.descricao = "";
      this.problemasSelecionados = [];
      this.sucesso = true;

      setTimeout(() => (this.sucesso = false), 3000);
    },
  },
};
</script>

<style scoped>
.reclamacoes {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  background: #333;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  color: #fff;
}

h2 {
  text-align: center;
  font-size: 24px;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

textarea {
  width: 580px;
  padding: 10px;
  font-size: 14px;
  border-radius: 5px;
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

.success {
  color: #2ecc71;
  text-align: center;
  font-size: 16px;
  margin-top: 20px;
}
</style>
