<template>
  <div>
    <h2>Login</h2>
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="email">Email</label>
        <input
          type="text"
          v-model="email"
          name="email"
          class="form-control"
          :class="{ 'is-invalid': submitted && !email }"
        >
        <div v-show="submitted && !email" class="invalid-feedback">Username is required</div>
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input
          type="password"
          v-model="password"
          name="password"
          class="form-control"
          :class="{ 'is-invalid': submitted && !password }"
        >
        <div v-show="submitted && !password" class="invalid-feedback">Password is required</div>
      </div>
      <div class="form-group">
        <button class="btn btn-primary" :disabled="loading">Login</button>

        <router-link to="/projects">Projects</router-link>
        <img
          v-show="loading"
          src="data:image/gif;base64,R0lGODlhEAAQAPIAAP///wAAAMLCwkJCQgAAAGJiYoKCgpKSkiH/C05FVFNDQVBFMi4wAwEAAAAh/hpDcmVhdGVkIHdpdGggYWpheGxvYWQuaW5mbwAh+QQJCgAAACwAAAAAEAAQAAADMwi63P4wyklrE2MIOggZnAdOmGYJRbExwroUmcG2LmDEwnHQLVsYOd2mBzkYDAdKa+dIAAAh+QQJCgAAACwAAAAAEAAQAAADNAi63P5OjCEgG4QMu7DmikRxQlFUYDEZIGBMRVsaqHwctXXf7WEYB4Ag1xjihkMZsiUkKhIAIfkECQoAAAAsAAAAABAAEAAAAzYIujIjK8pByJDMlFYvBoVjHA70GU7xSUJhmKtwHPAKzLO9HMaoKwJZ7Rf8AYPDDzKpZBqfvwQAIfkECQoAAAAsAAAAABAAEAAAAzMIumIlK8oyhpHsnFZfhYumCYUhDAQxRIdhHBGqRoKw0R8DYlJd8z0fMDgsGo/IpHI5TAAAIfkECQoAAAAsAAAAABAAEAAAAzIIunInK0rnZBTwGPNMgQwmdsNgXGJUlIWEuR5oWUIpz8pAEAMe6TwfwyYsGo/IpFKSAAAh+QQJCgAAACwAAAAAEAAQAAADMwi6IMKQORfjdOe82p4wGccc4CEuQradylesojEMBgsUc2G7sDX3lQGBMLAJibufbSlKAAAh+QQJCgAAACwAAAAAEAAQAAADMgi63P7wCRHZnFVdmgHu2nFwlWCI3WGc3TSWhUFGxTAUkGCbtgENBMJAEJsxgMLWzpEAACH5BAkKAAAALAAAAAAQABAAAAMyCLrc/jDKSatlQtScKdceCAjDII7HcQ4EMTCpyrCuUBjCYRgHVtqlAiB1YhiCnlsRkAAAOwAAAAAAAAAAAA=="
        >
      </div>
      <div v-if="error" class="alert alert-danger">{{error}}</div>
    </form>
  </div>
</template>

<script>
import { authService, router } from "../_services";

export default {
  data() {
    return {
      email: "",
      password: "",
      submitted: false,
      loading: false,
      returnUrl: "",
      error: ""
    };
  },
  created() {
    // log out user for now if navigations ends up on this page.
    authService.logout();

    // get return url from route parameters or default to '/'
    this.returnUrl = this.$route.query.returnUrl || "/";
  },
  methods: {
    handleSubmit(e) {
      this.submitted = true;
      const { email, password } = this;

      if (!(email && password)) {
        return;
      }

      this.loading = true;
      authService
        .login(email, password)
        .then(() => {
          this.loading = false;
          return router.push(this.returnUrl);
        })
        .catch(error => {
          this.error = error.message;
          this.loading = false;
        });
    }
  }
};
</script>