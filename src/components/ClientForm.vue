<template>
  <div class='form-wrapper'> 
    <h2>Вход в личный кабинет</h2>
    <p>Используйте логин и пароль из памятки</p>
    <form @submit.prevent='getData' class='form'>
      <label for='login'></label>
      <input v-model='login' class='form-input' id='login' type='text' name='login' placeholder='Логин' />
      <label for='password'></label>
      <input v-model='password' class='form-input' id='password' type='password' name='password' placeholder='Пароль' />
      <button type='submit'>Войти</button>
    </form>
  </div>
</template>

<script>
import FingerprintJS from '@fingerprintjs/fingerprintjs';

export default {
  name: 'ClientForm',
  data() {
    return {
      login: '',
      password: '',
    }
  },
  methods: {
    async getData() {
      const fingerprint = await this.getFingerprint();
      const IMEI = fingerprint.visitorId;
      const params = JSON.stringify({
        login: this.login,
        password: this.password,
        IMEI: IMEI,
        Name_app: "connect",
      });
      const result = await fetch(`https://host1.medsafe.tech:40443/api/client_login?json=${params}`);
      const data = await result.json();
      const id_login = data[0].id_login;
      const TK = data[0].TK;
      const docsDataParams = JSON.stringify({
        id_login: id_login,
        id_people: id_login,
        TK: TK,
        IMEI: IMEI,
        Name_app: "connect",
        Name_event: "list_load",
      })
      if (id_login != 0) {
        const secondQuery = await fetch(`https://host1.medsafe.tech:40443/api/test?json=${docsDataParams}`);
        const docsData = await secondQuery.json();
        this.$emit('submit', docsData.body, id_login, TK, IMEI)
      } else {
        console.log('Ошибка! Неподходящий формат данных')
      }
    },
    async getFingerprint() {
      const fpPromise = FingerprintJS.load()
      const fp = await fpPromise;
      const result = await fp.get();
      return result;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form-wrapper {
  max-width: 310px;
  margin: 0 auto;
}

h2 {
  font-size: 30px;
  font-weight: 700;
  margin: 0 0 10px;
}

.form {
  display: flex;
  flex-direction: column;
}

.form-input {
  display: block;
  max-width: 280px;
  border: 1px solid #d9d9d9;
  border-radius: 2px;
  box-shadow: none;
  color: #c2c2c2;
  height: 34px;
  padding: 6px 12px;
  font-size: 14px;
  margin-bottom: 10px;
}

button {
  background: #475168;
  color: #fff;
  font-weight: 600;
  text-transform: uppercase;
  border-radius: 4px;
  padding: 20px 40px;
  margin: 10px auto 0;
  transition: all .3s ease;
  border: none;
  width: 130px;
  cursor: pointer;
  letter-spacing: 0.04em;
}

button:hover,
button:focus {
  background: #48cfad;
}
</style>
