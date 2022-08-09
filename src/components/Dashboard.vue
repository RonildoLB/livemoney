<template>
<div>
  <h2 style="margin-bottom: 10px">Transações: </h2>
  <div id="burger-table" v-if="transacoes">
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Descrição:</div>
        <div>Valor:</div>
        <div>Categoria:</div>
        <div class="order-tipo">Tipo:</div>
        <div class="order-data">Data:</div>
        <div>Status:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="transacao in transacoes" :key="transacao.id">
        <template v-if="transacao.date === date2">
        <div class="order-number" >{{ transacao.id }}</div>
        <div class="valor">{{ transacao.description }}</div>
        <div class="valor">R$ {{ transacao.value.toFixed(2).toString().replace(".", ",") }}</div>
        <div class="valor">{{ transacao.category_id }}</div>
        <div class="order-tipo">{{ transacao.type }}</div>
        <div class="order-data">
          <div >{{ transacao.date }}</div>
        </div>
        <div class="order-status">
          <select name="status" class="status" @change="updateTrans($event, transacao.id)">
            <option value="1" :selected="transacao.status == 1">Aprovado</option>
            <option value="0" :selected="transacao.status == 0">Pendente</option>
          </select>
        </div>
        <div class="order-btn">
          <router-link to="/alterar">
          <button class="delete-btn" @click="updateTrans()">
            <img id="logo" src="/img/editar.png" alt="Editar">
          </button>
          </router-link>
          <button class="delete-btn" @click="deleteTrans(transacao.id); $emit('reloadPage')">
            <img id="logo" src="/img/lixeira.png" alt="Excluir">
          </button>
        </div>
        </template>
        <template v-else></template>
      </div>
    </div>
  </div>
  <div v-else>
    <div v-if="!exist">
    <h1><br><br>Não há transações!</h1>
    </div>
    <div v-else>
    <h1><br><br>Carregando...</h1>
    </div>
  </div>
</div>
</template>
<script>
  export default {
    name: "Dashboard",
    data() {
    return {
      reserva: [],
      login: "",
      iduser: ""
    }
  },
  props: {
    saldo_dia: Number,
    transacoes: Object,
    status: Object,
    date2: String,
    exist: Boolean,
  },
  emits: ['reloadPage'],
  methods: {
    getCookie(nome) {
      var nomeCookie = nome + "=";
      var ca = document.cookie.split(';');
      for(var i=0;i<ca.length;i++){
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if(c.indexOf(nomeCookie) == 0) return c.substring(nomeCookie.length, c.length);
      }
      return null;
    },
    async deleteTrans(id) {
      const req = await fetch(`https://teste2.flowcash.app/api/transactions/${id}`, {
        method: "DELETE",
        headers: {  "Authorization" : "Bearer "+this.login }
      });
    },
    async updateTrans() {

    },
  },
  mounted(){
    this.iduser = this.getCookie("idusuario");
    this.login = this.getCookie("loginusuario");
  }
}
</script>
  
<style scoped>
  h1 {
    text-align: center;
    font-size: 28px;
    color: #999;
  }

  .valor {
    font-weight: bold;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 20px;
    padding: 15px 0px 15px 0px
  }

  #burger-table {
    max-width: 1200px;
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    background: rgb(2,0,36);
background: linear-gradient(180deg, rgb(231, 230, 213) 0%, #f1eedd 100%);
    display: flex;
    flex-wrap: wrap;
  }

  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  .burger-table-row {
    width: 100%;
    padding: 0px 12px 0px 12px;
    font-weight: bold;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 20px;
  }

  #burger-table-heading div,
  .burger-table-row div {
    width: 17%;
  }

  .burger-table-row div {
    padding: 12px 0px 24px 0px;
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    
    width: 3%;
  }

  .burger-table-row .order-number {
    font-size: 12px;
    padding: 20px 0px 12px 0px;
  }

  #burger-table-heading .order-tipo,
  .burger-table-row .order-tipo {
    width: 10%;
  }

  .burger-table-row .order-tipo {
    padding: 15px 0px 12px 0px;
  }

  #burger-table-heading .order-data,
  .burger-table-row .order-data {
    width: 15%;
  }

  .burger-table-row .order-data {
    padding: 5px 0px 5px 0px;
  }

  #burger-table-heading .order-status,
  .burger-table-row .order-status {
    width: 115px;
  }

  .burger-table-row .order-status {
    padding: 12px 0px 12px 0px;
  }

  #burger-table-heading .order-btn,
  .burger-table-row .order-btn {
    width: 101px;
  }

  .burger-table-row .order-btn {
    padding: 12px 0px 12px 0px;
  }

  select {
    padding: 12px 6px;
    margin-right: 12px;
  }

  .delete-btn {
    background-color: transparent;
    border: transparent;
    margin: 5px;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }

  #nav-date a {
    text-shadow: 0 0 1em #ffc21b, 0 0 1em #ffc21b, 0 0 1em #ffc21b;
    color: #000;
    text-decoration: none;
    margin: 12px;
    transition: .5s;
  }

  #nav-date a:hover {
    color: #fcba03;
  }
  
</style>