<template>
  <div class="login row justify-content-center">
    <div class="col-md-4">
      <div class="card">
        <div class="card-heade">Login</div>
        <div class="card-body">
          <form @submit.prevent="authenticate">
            <div class="form-group row">
              <label for="email">Email</label>
              <input
                type="email"
                v-model="form.email"
                class="form-control"
                placeholder="Please enter email"
              />
            </div>
            <h2>{{form.email}}</h2>
            <div class="form-group row">
              <label for="password">Password</label>
              <input
                type="password"
                v-model="form.password"
                class="form-control"
                placeholder="Please enter password"
              />
            </div>
            <div class="form-group row">
              <input type="submit" value="Login" />
            </div>
            <div class="form-group row" v-if="authError">
              <p class="error">{{ authError }}</p>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { login } from "../../helpers/auth";

export default {
  name: "login",
  data() {
    return {
      form: {
        email: "",
        password: ""
      },
      errors: null
    };
  },
  methods: {
    authenticate() {
      this.$store.dispatch("login");

      login(this.form)
        .then(res => {
          this.$store.commit("loginSuccess", res);
          this.$router.push({ path: "/" });
        })
        .catch(err => {
          this.$store.commit("loginFailed", { err });
        });
    }
  },
  computed: {
    authError() {
      return this.$store.getters.authError;
    }
  }
};
</script>

<style scoped>
.error {
  text-align: center;
  color: red;
}
</style>