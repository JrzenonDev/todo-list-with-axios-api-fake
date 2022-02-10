<template>
  <b-row class="vh-100 vw-100 row-login">
    <b-col sm="5" class="d-flex justify-content-center align-items-center left-login">
      <div class="col-8">
        <h2>Faça o login</h2>
        <b-form>
          <b-form-group
            label="E-mail"
            label-for="email"
          >
            <b-form-input
              id="email"
              type="email"
              placeholder="seu@email.com.br"
              autocomplete="off"
              v-model="form.email"
            />
          </b-form-group>
          <b-form-group
            label="Senha"
            label-for="password"
          >
            <b-form-input
              id="password"
              type="password"
              placeholder="Digite sua senha"
              v-model="form.password"
            />
          </b-form-group>
        </b-form>
      </div>
    </b-col>
    <b-col sm="7"></b-col>
  </b-row>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      form: {
        email: '',
        password: ''
      }
    }
  }
}
</script>

<style>
  *,
  *::after,
  *::before {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    text-decoration: none;
  }
  .row-login {
    margin-left: 0;
  }
  .left-login {
    background-color: #f2f2f2;
  }
</style>
