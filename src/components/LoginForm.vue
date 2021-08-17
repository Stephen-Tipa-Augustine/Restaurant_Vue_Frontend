<template>
<div>
  <div v-if="error" class="alert alert-danger">{{error}}</div>
    <!-- login form -->
    <form @submit.prevent="login" class="user">
      <div class="form-group">
        <label>Username:
          <input type="text" class="form-control form-control-user" placeholder="e.g Steve" required autofocus v-model="form.username">
        </label>
      </div>
      <div class="form-group">
        <label>Password:
          <input type="password" class="form-control form-control-user" placeholder="Password" required v-model="form.password">
        </label>
      </div>
      <div class="form-group">
        <div class="custom-control custom-checkbox small">
          <input type="checkbox" class="custom-control-input" id="customCheck">
          <label class="custom-control-label" for="customCheck">Remember Me</label>
        </div>
      </div>
      <input type="submit" value="Login" class="btn btn-info btn-user btn-block">

      <!-- Modal to show the loading effect during login -->
      <div class="modal align-content-center" id="modal">
          <div class="modal-content">
            <div style="width: 100%; display: block;">
              <div style="text-align: center">
                <div class="spinner-border text-success" role="status">
                  <span class="sr-only"></span>
                </div>
              </div>
            </div>
          </div>
      </div>

      <div class="container">
        <div class="row">
          <div class="col-12">
            <p class="text-info" v-if="loggedIn">
              Login successful! Close this modal to explore.
            </p>
          </div>
        </div>
      </div>
    </form>
</div>
</template>

<script>
export default {
name: "LoginForm",
  data() {
    return {
      form: {
        username: '',
        password: '',
      },
      error: null,
      loggedIn: false,
      users: [],
    };
  },
  methods: {
    loadingModal() {
      const modal = document.getElementById('modal');
      modal.style.display = 'block';
      // When the user clicks anywhere outside of the modal, close it
      // eslint-disable-next-line func-names
      window.onclick = function (event) {
        // eslint-disable-next-line eqeqeq
        if (event.target === modal) {
          modal.style.display = 'none';
        }
      };
    },
    login() {
      const modal = document.getElementById('modal');
      modal.style.display = 'block';
      console.log('User is trying to sign in')
      this.axios.post('http://localhost:9000/api/auth/signin', this.form)
          .then((res) => {
               //Perform Success Action
            console.log(res.data)
            if (res.data.accessToken != null) {
              localStorage.setItem('user', JSON.stringify(res.data));
              console.log('Login succeeded, proceeding to homepage')
              console.log(res.data.message)
              modal.style.display = 'none';
              this.$emit('logged-in');
              this.loggedIn = true;
            }
           })
           .catch((error) => {
               // error.response.status Check status code
             modal.style.display = 'none';
             console.log(error.message)
           }).finally(() => {
               //Perform action in always
              console.log('axios successfully invoked')
           });
    },
  },
}
</script>

<style scoped>

</style>
