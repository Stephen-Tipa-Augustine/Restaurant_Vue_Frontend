<template>
<div class="container-scroller">
      <div class="main-panel">
        <!-- ======= Top Bar ======= -->
        <section id="topbar" class="d-flex align-items-center fixed-top topbar-transparent">
          <div class="container-fluid container-xl d-flex align-items-center justify-content-center justify-content-lg-start">
            <i class="bi bi-phone d-flex align-items-center"><span>+256 775 567851</span></i>
            <i class="bi bi-clock ms-4 d-none d-lg-flex align-items-center"><span>Mon-Sat: 8:00 AM - 23:00 PM</span></i>
          </div>
        </section>

        <section id="about" class="about">
          <div class="container-fluid">
            <div class="row">
              <div class="col-lg-7 d-flex flex-column justify-content-center align-items-stretch">
                <div class="content">
                  <h3>Thanks for visiting our restaurant. We humbly appreciate.</h3>
                  <h4>
                    Go back to home: <router-link to="/">Home</router-link>
                  </h4>
                </div>
              </div>
            </div>
          </div>
        </section>

        <Footer />
      </div>
    </div>
</template>

<script>
import Footer from "@/components/Footer";
export default {
  name: "Logout",
  components: {Footer}
}
</script>

<style scoped>

</style>
