<template>
  <div class="text-center">
    <WelcomeHeader/>
    <div class="container-fluid background-container">
      <div class="row">
        <div class="col-12">
          <div class="modal-dialog">
            <div class="col-sm-9 main-section text-center">
              <div class="modal-content">
                <h3>LOGIN</h3>
                <div class="col-12 form-input">
                  <form @submit.prevent="login({name: 'home'})">
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <div class="input-group-text">
                          <i class="fa fa-envelope" aria-hidden="true"></i>
                        </div>
                      </div>
                      <input type="email" class="form-control" 
                        v-model="email" placeholder="Email"
                        required="true"/>
                    </div>
                    <div class="form-group input-group">
                      <div class="input-group-prepend">
                        <div class="input-group-text">
                          <i class="fa fa-key" aria-hidden="true"></i>
                        </div>
                      </div>
                      <input type="password" class="form-control" 
                        v-model="password" placeholder="Password" required="true"/>
                    </div>
                    <div v-if="show">
                      <p class="error">
                        <i class="fa fa-exclamation-circle" aria-hidden="true"></i>
                        {{ error }}
                      </p>
                    </div>
                    <div class="form-group">
                      <button type="submit" class="form-control btn btn-custom">
                        Login
                        <i class="fa fa-user" aria-hidden="true"></i>
                      </button>
                    </div>
                  </form>
                  <router-link to="/register" class="register-link">
                    Create New Account
                  </router-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import WelcomeHeader from "@/components/WelcomeHeader.vue";
import LoginService from "@/services/LoginService.js";

export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
      error: null,
      show: false
    };
  },
  components: {
    WelcomeHeader
  },
  methods: {
    async login(route) {
      try {
        const response = await LoginService.login({
          email: this.email,
          password: this.password
        });
        this.$store.dispatch("setToken", response.data.token);
        this.$store.dispatch("setUser", response.data.user);
        this.$router.push(route);
      } catch (error) {
        (this.show = true), (this.error = error.response.data.message);
      }
    }
  }
};
</script>

<style scoped>
.background-container {
  position: relative;
  width: 100%;
  min-height: 100vh;
  background-color: transparent;
  background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.6)),
    url("../assets/landing.jpg") no-repeat center top;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -ms-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  background-attachment: fixed;
  overflow: hidden;
  padding-bottom: 40px;
}
.main-section {
  margin: 0 auto;
  margin-top: 130px;
  padding: 0;
}
.modal-content {
  background: linear-gradient(rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.6));
  padding: 0 18px;
  border-radius: 10px;
  border: 1px solid #420906;
}
h3 {
  padding-top: 25px;
  letter-spacing: 1px;
}
.error {
  font-size: 14px;
  letter-spacing: 1px;
  padding-bottom: 5px;
}
form {
  padding-top: 25px;
}
.form-group {
  margin-bottom: 25px;
  font-size: 18px;
}
.form-group .fa {
  color: #fbe9e7;
}
.form-control:focus {
  border-color: #fff;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(255, 0, 0, 0.6);
}
.input-group-text {
  background-color: transparent;
}
.form-group input {
  color: #fff;
  background-color: transparent;
  height: 42px;
  border-radius: 5px;
  border: 1px solid #fbe9e7;
  letter-spacing: 2px;
}
.form-input {
  padding-bottom: 20px;
}
/* Chrome/Opera/Safari */
::-webkit-input-placeholder {
  color: #fbe9e7;
}
/* Firefox 19+ */
::-moz-placeholder {
  color: #fbe9e7;
}
/* IE 10+ */
:-ms-input-placeholder {
  color: #fbe9e7;
}
/* Firefox 18- */
:-moz-placeholder {
  color: #fbe9e7;
}
/* Most modern browsers support this now. */
::placeholder {
  color: #fbe9e7;
}
.btn.btn-custom {
  background-color: #420906;
  letter-spacing: 1px;
  color: #fff;
  font-size: 20px;
  border: 1px solid #fbe9e7;
  border-radius: 5px;
  overflow: hidden;
}
.btn.btn-custom:hover {
  background-color: transparent;
}
.btn.btn-custom > i {
  padding-right: 2px;
}
.register-link {
  color: #fff;
  letter-spacing: 2px;
}
</style>
