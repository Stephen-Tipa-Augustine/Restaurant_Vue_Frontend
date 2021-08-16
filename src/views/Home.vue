<template>
  <div>
    <!-- ======= Top Bar ======= -->
  <section id="topbar" class="d-flex align-items-center fixed-top topbar-transparent">
    <div class="container-fluid container-xl d-flex align-items-center justify-content-center justify-content-lg-start">
      <i class="bi bi-phone d-flex align-items-center"><span>+256 775 567851</span></i>
      <i class="bi bi-clock ms-4 d-none d-lg-flex align-items-center"><span>Mon-Sat: 8:00 AM - 23:00 PM</span></i>
    </div>
  </section>

  <!-- ======= Header ======= -->
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
              <li><a href="#login" data-bs-toggle="modal" v-if="!auth">Login</a></li>
              <li><a href="#register" data-bs-toggle="modal" v-if="!auth">Register</a></li>
              <li><a @click="logout()" v-if="auth" style="cursor: pointer;">Logout</a></li>
              <li>
                <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#login">
                  Launch static backdrop modal
                </button>
              </li>
            </ul>
          </li>
          <li><a class="nav-link scrollto" href="#contact">Contact</a></li>
        </ul>
        <i class="bi bi-list mobile-nav-toggle"></i>


      </nav><!-- .navbar -->

      <a href="#book-a-table" class="book-a-table-btn scrollto">Book a table</a>

    </div>

    <!-- Modal -->
    <div class="modal fade" id="login" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Login Now</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <LoginForm v-on:logged-in="initialize" />
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="register" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel1">Register Now</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <RegisterForm v-on:registerd="initialize"/>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>


  </header><!-- End Header -->

  <!-- ======= Hero Section ======= -->
    <Hero />
  <!-- End Hero -->

  <main id="main">

    <!-- ======= About Section ======= -->
    <About />
    <!-- End About Section -->

    <!-- ======= Whu Us Section ======= -->
    <WhyUs />
    <!-- End Whu Us Section -->

    <!-- ======= Menu Section ======= -->
    <Menu />
    <!-- End Menu Section -->

    <!-- ======= Specials Section ======= -->
    <Specials />
    <!-- End Specials Section -->

    <!-- ======= Events Section ======= -->
    <Events />
    <!-- End Events Section -->

    <!-- ======= Book A Table Section ======= -->
    <Table :user="user" />
    <!-- End Book A Table Section -->

    <!-- ======= Gallery Section ======= -->
    <Gallery />
    <!-- End Gallery Section -->

    <!-- ======= Chefs Section ======= -->
    <Chefs />
    <!-- End Chefs Section -->

    <!-- ======= Testimonials Section ======= -->
    <Testimonials />
    <!-- End Testimonials Section -->

    <!-- ======= Contact Section ======= -->
    <Contact :user="user" />
    <!-- End Contact Section -->

  </main><!-- End #main -->

  <!-- ======= Footer ======= -->
    <Footer />
  <!-- End Footer -->

  <a href="#" class="back-to-top d-flex align-items-center justify-content-center"><i class="bi bi-arrow-up-short"></i></a>
  </div>
</template>

<script>
// @ is an alias to /src

import Hero from "@/components/Hero";
import About from "@/components/About";
import WhyUs from "@/components/WhyUs";
import Menu from "@/components/Menu";
import Specials from "@/components/Specials";
import Events from "@/components/Events";
import Table from "@/components/Table";
import Gallery from "@/components/Gallery";
import Chefs from "@/components/Chefs";
import Testimonials from "@/components/Testimonials";
import Contact from "@/components/Contact";
import Footer from "@/components/Footer";
import RegisterForm from "@/components/RegisterForm";
import LoginForm from "@/components/LoginForm";
export default {
  name: 'Home',
  components: {
    LoginForm,
    RegisterForm,
    Footer, Contact, Testimonials, Chefs, Gallery, Table, Events, Specials, Menu, WhyUs, About, Hero},
  data() {
    return {
      isAdmin: false,
      user: null,
      auth: false
    }
  },
  created() {
    this.initialize()
  },
  methods: {
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
    logout() {
      localStorage.removeItem('user');
      this.$router.replace({ name: 'Logout' });
    }
  },
}
</script>
