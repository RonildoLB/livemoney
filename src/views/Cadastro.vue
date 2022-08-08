<template>
  <div>
    <Navbarlimpo />
    <div class="main-container">
      <center><h1>Cadastro:</h1></center>
      <CadasForm />
    </div>
  </div>
</template>

<script>
import CadasForm from '../components/CadasForm.vue'
import Navbarlimpo from '../components/Navbarlimpo.vue'

export default {
  name: 'Cadastro',
  components: {
    CadasForm,
    Navbarlimpo
  }
}
</script>