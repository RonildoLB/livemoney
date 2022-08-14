<template>
  <div>
    <div class="main-banner" v-if="trans_exist">
      <div class="item" style="color: rgb(180, 245, 178);">
        <h2>Receitas: R$ {{ receita_dia.toFixed(2).replace(".", ",") }}</h2>
      </div>
      <div class="item" style="color: rgb(241, 185, 185);">
        <h2>Despesas: R$ {{ despesa_dia.toFixed(2).replace(".", ",") }}</h2>
      </div>
      
      <div class="item"><hr><br><h2>Saldo: R$ {{ saldo_dia.toFixed(2).replace(".", ",") }}</h2></div>
    </div>
    <div v-else class="main-banner">
      <div class="item">
        <h2><br>Carregando...</h2>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Banner",
    data() {
    return {
      inicial_dia: null,
      final_dia: null
    }
  },
  props: {
    saldo_dia: Number,
    receita_dia: Number,
    despesa_dia: Number,
    trans_exist: Boolean
  }
}
</script>

<style scoped>

  .item {
    justify-content: center;
    text-align: center;
    color: aliceblue;
    padding: 10px 0px 0px 0px;
  }

  .main-banner {
    background-color: #222;
    height: 150px;
  }


</style>