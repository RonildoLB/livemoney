<template>
<div>
  <h2 style="margin-bottom: 10px; text-align: center">Transações: </h2>
  <div id="burger-table" v-if="trans_exist">
    <div id="burger-table-rows">
      <div class="burger-table-row"  v-for="transacao in transacoes" :key="transacao.id">
        <template v-if="(transacao.date === date2) || !mostrar">
          <div class="trans-main">
            <div class="trans-row1">
              <div class="trans-col">
                <div v-show="!transacao.status" class="trans-status" @click="alternar(transacao)">
                  <img class="pend-icon" src="/img/pendente.svg" alt="Status">
                </div>
                <div v-show="transacao.status" class="aprov-status" @click="alternar(transacao)">
                  <img class="pend-icon" src="/img/aprovado.svg" alt="Status">
                </div>
              </div>
              <div class="trans-col">
                <div class="trans-descri">
                  <div class="titul-text">Descrição</div>
                  <div class="descri-text">{{transacao.description}}</div>
                </div>
                <div class="trans-valor">
                  <div class="burger-table-row">
                    <div>
                      <div class="titul-text">Valor</div>
                      <div v-if="transacao.type == this.receita" class="descri-valor-r">
                        R$ {{ transacao.value.toFixed(2).toString().replace(".", ",") }}
                      </div>
                      <div v-if="transacao.type == this.despesa" class="descri-valor-d">
                        R$ -{{ transacao.value.toFixed(2).toString().replace(".", ",") }}
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="trans-col">
                <div class="trans-descri">
                  <div class="titul-text">Data</div>
                  <div class="descri-text">{{ transacao.date }}</div>
                </div>
                <div class="trans-descri">
                  <div class="titul-text">Categoria</div>
                  <div class="descri-text">{{ transacao.category_id }}</div>
                </div>
                <div class="order-btn">
                  <div>
                    <router-link to="/alterar">
                      <button class="delete-btn" @click="updateTrans()">
                        <img class="deleteicons" src="/img/editar.svg" alt="Editar">
                      </button>
                    </router-link>
                  </div>
                  <div>
                    <button class="delete-btn" @click="deleteTrans(transacao.id); $emit('reloadPage')">
                      <img class="deleteicons" src="/img/lixeira.svg" alt="Excluir">
                    </button>
                  </div>
                </div>
              </div>
            </div>
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
      iduser: "",
      aprovado: false,
      receita: "receita",
      despesa: "despesa"
    }
  },
  props: {
    saldo_dia: Number,
    transacoes: Object,
    status: Object,
    date2: String,
    exist: Boolean,
    mostrar: Boolean,
    trans_exist: Boolean
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
    async alternar(transacao) {
      transacao.status = !transacao.status;
    }
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

  .trans-main {
    background: linear-gradient(180deg, rgb(255, 255, 255) 0%, rgb(245, 245, 237) 100%);
    min-width: 100px;
    min-height: 100px;
    margin: 10px;
    border-radius: 10px 10px 10px 10px;
    box-shadow: 0 0 8px 3px rgba(0, 0, 0, 0.384);
  }

  .trans-row1 {
    display: flex;
    flex-direction: row;
    justify-content:flex-start;
    align-items:flex-start;
  }

  .trans-col {
    display: inline-block;
  }

  .trans-status {
    display: inline-block;
    padding:3px 5px;
    background-color: rgb(122, 21, 100);
    border-radius: 10px 0px 10px 10px;
    border: 1px solid rgb(196, 39, 162);
    cursor: pointer;
  }

  .aprov-status {
    display: inline-block;
    padding:3px 5px;
    background-color: rgb(24, 122, 21);
    border-radius: 10px 0px 10px 10px;
    border: 1px solid rgb(39, 196, 47);
    cursor: pointer;
  }

  .trans-descri {
    padding: 8px 12px;
    max-width: 25ch;
  }
  .descri-text {
    padding-top: 2px;
    font-size: 14px;
  }
  .titul-text {
    font-size: 10px;
    color:#666
  }

  .trans-valor {
    padding: 10px 0px;
  }
  .descri-valor-r {
    font-size: 36px;
    color: rgb(73, 179, 69);
  }
  .descri-valor-d {
    font-size: 36px;
    color: rgb(206, 85, 85);
  }

  .pend-icon {
    height: 15px;
  }

  .valor {
    font-weight: bold;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 20px;
    padding: 15px 0px 15px 0px
  }

  #burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .burger-table-row {
    width: 100%;
    padding: 0px 12px 0px 12px;
    font-weight: bold;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 20px;
  }

  .deleteicons {
    height: 25px;
  }

  .order-btn {
    padding: 8px 12px;
    display: flex;
    flex-direction: row;
    justify-content:flex-start;
    align-items:flex-start;
  }

  .delete-btn {
    background-color: transparent;
    border: transparent;
    border-radius: 10px;
    padding: 5px;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: #ccc;
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