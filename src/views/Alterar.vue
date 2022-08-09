<template>
  <div>
    <Navbarlimpo/>
    <div class="main-container">
      <center><h1>Alterar transação:</h1></center>
      <BurgerForm />
    </div>
  </div>
</template>

<script>
import BurgerForm from '../components/BurgerForm'
import Navbarlimpo from '../components/Navbarlimpo.vue'

export default {
  name: 'Alterar',
  components: {
    BurgerForm,
    Navbarlimpo
  }
}
</script>
