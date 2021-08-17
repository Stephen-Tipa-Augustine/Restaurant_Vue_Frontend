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
    <Header />
  <!-- End Header -->

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

    <!-- ======= Bookings Section ======= -->
    <Bookings v-if="isAdmin" />
    <!-- End Bookings Section -->

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
import Header from "@/components/Header";
import Bookings from "@/components/Bookings";
export default {
  name: 'Home',
  components: {
    Bookings,
    Header, Footer, Contact, Testimonials, Chefs, Gallery, Table, Events, Specials, Menu, WhyUs, About, Hero},
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
