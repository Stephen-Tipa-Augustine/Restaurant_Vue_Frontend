<template>
<div>
  <div v-if="error" class="alert alert-danger">{{error}}</div>
    <!-- login form -->
    <form @submit.prevent="register" class="user">
      <div class="form-group">
        <label>Username:
          <input type="text" class="form-control form-control-user" placeholder="e.g Steve" required v-model="form.username">
        </label>
      </div>
      <div class="form-group">
        <label>Email:
          <input type="email" class="form-control form-control-user" placeholder="Enter Email Address..." required autofocus v-model="form.email">
        </label>
      </div>
      <div class="form-group">
        <label>Password:
          <input type="password" class="form-control form-control-user" placeholder="Password" required v-model="form.password">
        </label>
      </div>
      <div class="form-group">
        <label>User Role:
          <select name="roles" id="roles" class="form-control form-control-user" required v-model="userRole">
            <option value="" disabled>Choose</option>
            <option value="user">User</option>
            <option value="admin">Admin</option>
            <option value="mod">Mod</option>
          </select>
        </label>
      </div>

      <div class="form-group">
        <div class="custom-control custom-checkbox small">
          <input type="checkbox" class="custom-control-input" id="customCheck2" v-model="termsAccepted">
          <label class="custom-control-label" for="customCheck2">I Accept Terms and Conditions</label>
        </div>
      </div>
      <button type="submit" :disabled="!termsAccepted"
        class="btn btn-info btn-user btn-block" style="margin-top: 10px">
          <span>
            <p style="text-align: center; display: inline; font-size: large">
              Register
            </p>
            <div style="display: inline; margin-left: 100px" v-if="registering">
              <div class="spinner-border text-white"
                   role="status" style="margin: auto">
                <span class="sr-only">Loading...</span>
              </div>
            </div>
          </span>
        </button>
      <div class="container">
        <div class="row">
          <div class="col-12">
            <p class="text-info" v-if="registered">
              Registration successful! Close this modal and login with your new account.
            </p>
          </div>
        </div>
      </div>
    </form>
</div>
</template>

<script>
export default {
  name: "RegisterForm",
  data() {
    return {
      form: {
        username: '',
        email: '',
        password: '',
        role: []
      },
      termsAccepted: false,
      registering: false,
      registered: false,
      userRole: '',
      error: null,
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
    register() {
      this.registering = true;
      this.form.role.push(this.userRole)
      console.log(this.form)
      this.axios.post('http://localhost:9000/api/auth/signup', this.form)
          .then((res) => {
               //Perform Success Action
            console.log(res.data)
            if (res.data.message === 'User registered successfully!') {
              console.log('signup succeeded, proceed to login into your new account')
              this.registered = true;
              this.$emit('registered');
            }
           })
           .catch((error) => {
               // error.response.status Check status code
             console.log(error.message)
           }).finally(() => {
               //Perform action in always
              console.log('axios successfully invoked')
              this.registering = false
           });
      console.log('User is trying to sign in')
    },
  },
}
</script>
