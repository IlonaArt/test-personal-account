<template>
  <div id='app'>
    <div class='container'>
      <ClientForm v-if='isFormVisible' @submit='onSubmit' />
      <DocsList v-else :docs='docs' :id_login='id_login' :TK='TK' :IMEI='IMEI' />
  </div>
  </div>
</template>

<script>
import ClientForm from './components/ClientForm.vue'
import DocsList from './components/DocsList.vue'

export default {
  name: 'App',
  data() {
    return {
      docs: [],
      isFormVisible: true,
      id_login: null,
      TK: '',
      IMEI: '',
    }
  },
  components: {
    ClientForm,
    DocsList,
  },
  methods: {
    onSubmit(data, id_login, TK, IMEI) {
      this.docs = data;
      this.isFormVisible = !this.isFormVisible;
      this.id_login = id_login;
      this.TK = TK;
      this.IMEI = IMEI;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}

html {
  height: 100%;
}

body {
  margin: 0;
}

.container {
  margin: 0 auto;
  padding: 0 40px;
}
</style>
