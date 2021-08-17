<template>
<header id="header" class="fixed-top d-flex align-items-center">
    <div class="container-fluid container-xl d-flex align-items-center justify-content-between">

      <div class="logo me-auto">
        <h1><a href="/">People's Restaurant</a></h1>
        <!-- Uncomment below if you prefer to use an image logo -->
        <!-- <a href="index.html"><img src="assets/img/logo.png" alt="" class="img-fluid"></a>-->
      </div>

      <nav id="navbar" class="navbar order-last order-lg-0">
        <ul>
          <li><a class="nav-link scrollto active" href="#hero">Home</a></li>
          <li><a class="nav-link scrollto" href="#about">About</a></li>
          <li><a class="nav-link scrollto" href="#menu">Menu</a></li>
          <li><a class="nav-link scrollto" href="#specials">Specials</a></li>
          <li><a class="nav-link scrollto" href="#events">Events</a></li>
          <li><a class="nav-link scrollto" href="#chefs">Chefs</a></li>
          <li><a class="nav-link scrollto" href="#gallery">Gallery</a></li>
          <li class="dropdown"><a href="#"><span>Account</span> <i class="bi bi-chevron-down"></i></a>
            <ul>
              <li><a href="#" @click="openModal('login')" v-if="!auth">Login</a></li>
              <li><a href="#" @click="openModal('register')" v-if="!auth">Register</a></li>
              <li><a @click="logout()" v-if="auth" style="cursor: pointer;">Logout</a></li>
            </ul>
          </li>
          <li><a class="nav-link scrollto" href="#contact">Contact</a></li>
          <li><a class="nav-link scrollto" href="#bookings" v-if="isAdmin">Bookings</a></li>
        </ul>
        <i class="bi bi-list mobile-nav-toggle"></i>


      </nav><!-- .navbar -->

      <a href="#book-a-table" class="book-a-table-btn scrollto">Book a table</a>

    </div>

    <!-- The Modal -->
    <div id="login" class="modal">

      <!-- Modal content -->
      <div class="modal-content">
        <div class="modal-header">
          <span class="close" @click="closeModal('login')">&times;</span>
          <h2>Login Now</h2>
        </div>
        <div class="modal-body">
          <LoginForm v-on:logged-in="initialize" />
        </div>
      </div>

    </div>


    <!-- The Modal -->
    <div id="register" class="modal">

      <!-- Modal content -->
      <div class="modal-content">
        <div class="modal-header">
          <span class="close" @click="closeModal('register')">&times;</span>
          <h2>Register Now</h2>
        </div>
        <div class="modal-body">
          <RegisterForm v-on:registerd="initialize" />
        </div>
      </div>

    </div>


  </header>
</template>

<script>
import LoginForm from "@/components/LoginForm";
import RegisterForm from "@/components/RegisterForm";
export default {
name: "Header",
  components: {RegisterForm, LoginForm},
  data() {
  return {
    isAdmin: false,
    user: null,
    auth: false,
    }
  },
  created() {
  this.initialize()
  },
  methods: {
  openModal(id) {
    const modal  = document.getElementById(id);
    modal.style.display = "block";

    // When the user clicks anywhere outside of the modal, close it
    window.onclick = (event) =>  {
      if (event.target === modal) {
        modal.style.display = "none";
      }
    }
  },
  closeModal(id) {
    const modal  = document.getElementById(id);
    modal.style.display = "none";
  },

  logout() {
      localStorage.removeItem('user');
      this.$router.replace({ name: 'Logout' });
    },

  initialize() {
      this.user = JSON.parse(localStorage.getItem('user'));
      if (this.user != null) {
        this.auth = (this.user.accessToken != null);
        this.user.roles.forEach((role) => {
        if (role === 'ROLE_ADMIN') {
          this.isAdmin = true
        }
      });
      }
    },
  },
}
</script>

<style scoped>
body {font-family: Arial, Helvetica, sans-serif;}

/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
  position: relative;
  background-color: #fefefe;
  margin: auto;
  padding: 0;
  border: 1px solid #888;
  width: 50%;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2),0 6px 20px 0 rgba(0,0,0,0.19);
  -webkit-animation-name: animatetop;
  -webkit-animation-duration: 0.4s;
  animation-name: animatetop;
  animation-duration: 0.4s
}

/* Add Animation */
@-webkit-keyframes animatetop {
  from {top:-300px; opacity:0}
  to {top:0; opacity:1}
}

@keyframes animatetop {
  from {top:-300px; opacity:0}
  to {top:0; opacity:1}
}

/* The Close Button */
.close {
  color: white;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}

.modal-header {
  padding: 2px 16px;
  background-color: #5cb85c;
  color: white;
}

.modal-body {padding: 2px 16px;}

.modal-footer {
  padding: 2px 16px;
  background-color: #5cb85c;
  color: white;
}
</style>
