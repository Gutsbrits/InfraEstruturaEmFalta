<template>
  <div class="visualizar">
    <h2>Suas Reclamações</h2>
    <ul v-if="reclamacoes.length">
      <li v-for="(reclamacao, index) in reclamacoes" :key="index" class="reclamacao-item">
        <div class="reclamacao-header">
          <strong>{{ reclamacao.data.descricao }}</strong>
          <span class="reclamacao-data">{{ reclamacao.data.data }}</span>
        </div>
        <p class="descricao">{{ reclamacao.data.problemas }}</p>
        <button @click="excluirReclamacao(reclamacao.key)" class="delete-btn">Excluir</button>
      </li>
    </ul>
    <p v-else class="no-reclame">Você ainda não registrou reclamações.</p>
  </div>
</template>

<script>

import Localbase from 'localbase'


export default {
  data() {
    return {
      reclamacoes: [],
      db:new Localbase('ief')
    };
  },
  async mounted() {
    await this.inicio()
  },
  methods: {
    async excluirReclamacao(index) {
      if (confirm("Tem certeza que deseja excluir esta reclamação?")) {
        await this.db.collection('reclamacoes').doc(index).delete()
        await this.inicio()
      }
    },
      async inicio(){
       this.reclamacoes= await this.db.collection('reclamacoes').get({keys: true})

      }
      
  },
};
</script>

<style scoped>
.visualizar {
  max-width: 800px;
  margin: 40px auto;
  padding: 30px;
  background: #2c2c2c;
  border-radius: 12px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
  color: #fff;
}

h2 {
  text-align: center;
  font-size: 26px;
  color: #f39c12;
  margin-bottom: 20px;
}

ul {
  list-style: none;
  padding: 0;
}

.reclamacao-item {
  background-color: #444;
  padding: 20px;
  border: 1px solid #666;
  border-radius: 8px;
  margin-bottom: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.reclamacao-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.reclamacao-header {
  font-size: 18px;
  color: #f39c12;
  font-weight: bold;
  margin-bottom: 8px;
}

.reclamacao-data {
  font-size: 14px;
  color: #aaa;
}

.descricao {
  font-size: 16px;
  color: #ddd;
  margin: 10px 0;
}

.delete-btn {
  background-color: #e74c3c;
  color: white;
  padding: 8px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s ease;
  margin-top: 10px;
}

.delete-btn:hover {
  background-color: #c0392b;
}

.no-reclame {
  text-align: center;
  font-size: 18px;
  color: #888;
  margin-top: 20px;
}
</style>