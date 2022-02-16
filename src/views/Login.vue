<template>
  <b-row class="vh-100 vw-100 row-login">
    <b-col sm="5" class="d-flex justify-content-center align-items-center left-login">
      <div class="col-8">
        <h2 class="text-center mb-5 title-login">Faça o login</h2>
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
              v-model.trim="$v.form.email.$model"
              :state="getValidation('email')"
            />
          </b-form-group>
          <b-form-group
            label-for="password"
          >
            <label for="password" class="d-flex justify-content-between">
              Senha
              <small><a href="#">Esqueceu sua senha?</a></small>
            </label>
            <b-form-input
              id="password"
              type="password"
              placeholder="Digite sua senha"
              v-model.trim="$v.form.password.$model"
              :state="getValidation('password')"
            />
          </b-form-group>
          <b-button
            type="button"
            variant="primary"
            block
            @click="login"
          >
            <i class="fas fa-sign-in-alt" /> Entrar
          </b-button>
          <hr />
          <b-button
            type="button"
            variant="outline-secondary"
            block
            @click="register"
          >
            <i class="fas fa-use-plus" /> Não tenho conta
          </b-button>
        </b-form>
      </div>
    </b-col>
    <b-col class="d-flex justify-content-center align-items-center left-login" sm="7">
      <img class="img-login" src="../assets/images/login.svg" alt="Login" />
    </b-col>
  </b-row>
</template>

<script>
import { required, minLength, email } from 'vuelidate/lib/validators'

export default {
  name: 'Login',
  data () {
    return {
      form: {
        email: '',
        password: ''
      }
    }
  },
  validations: {
    form: {
      email: {
        required,
        email
      },
      password: {
        required,
        minLength: minLength(6)
      }
    }
  },
  methods: {
    login () {
      this.$v.$touch()
      if (this.$v.$error) {
        return true
      }
    },
    register () {},
    getValidation (field) {
      if (this.$v.$dirty === false) {
        return null
      }
      return !this.$v.form[field].$error
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
  .title-login {
    font-weight: bold;
  }
  .img-login {
    width: 600px;
    height: 600px;
  }
</style>
