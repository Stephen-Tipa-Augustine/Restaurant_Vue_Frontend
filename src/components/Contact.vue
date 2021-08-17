<template>
<section id="contact" class="contact">
      <div class="container">

        <div class="section-title">
          <h2><span>Contact</span> Us</h2>
          <p>Reach us any time, our team works hard to respond to any of your concern or needs.</p>
        </div>
      </div>

      <div class="container mt-5">

        <div class="info-wrap">
          <div class="row">
            <div class="col-lg-3 col-md-6 info">
              <i class="bi bi-geo-alt"></i>
              <h4>Location:</h4>
              <p>A108 kampala Road<br>Kampala City, Uganda</p>
            </div>

            <div class="col-lg-3 col-md-6 info mt-4 mt-lg-0">
              <i class="bi bi-clock"></i>
              <h4>Open Hours:</h4>
              <p>Monday-Saturday:<br>11:00 AM - 2300 PM</p>
            </div>

            <div class="col-lg-3 col-md-6 info mt-4 mt-lg-0">
              <i class="bi bi-envelope"></i>
              <h4>Email:</h4>
              <p>info@example.com<br>contact@example.com</p>
            </div>

            <div class="col-lg-3 col-md-6 info mt-4 mt-lg-0">
              <i class="bi bi-phone"></i>
              <h4>Call:</h4>
              <p>+1 5589 55488 51<br>+1 5589 22475 14</p>
            </div>
          </div>
        </div>

        <form @submit.prevent="sendMessage" method="post" role="form" class="php-email-form">
          <div class="row">
            <div class="col-md-6 form-group">
              <input type="text" name="name" class="form-control" id="name" placeholder="Your Name" required v-model="form.name">
            </div>
            <div class="col-md-6 form-group mt-3 mt-md-0">
              <input type="email" class="form-control" name="email" id="email" placeholder="Your Email" required v-model="form.email">
            </div>
          </div>
          <div class="form-group mt-3">
            <input type="text" class="form-control" name="subject" id="subject" placeholder="Subject" required v-model="form.subject">
          </div>
          <div class="form-group mt-3">
            <textarea class="form-control" name="message" rows="5" placeholder="Message" required v-model="form.message"></textarea>
          </div>

          <div class="container">
            <div class="row justify-content-center">
              <div class="col-12">
                <div v-if="registering" style="width: fit-content; margin: auto">
                  <div class="spinner-border text-success"
                       role="status">
                    <span class="sr-only"></span>
                  </div>
                </div>
                <div class="text-warning text-center" v-if="error !== null">{{error}}</div>
                <div class="text-info text-center" v-if="registered">Your message has been sent. Thank you!</div>
              </div>
            </div>
          </div>
          <div class="text-center"><button type="submit">Send Message</button></div>
        </form>

      </div>
    </section>
</template>

<script>
export default {
  name: "Contact",
  data() {
    return {
      form: {
        name: '',
        email: '',
        subject: '',
        message: ''
      },
      error: null,
      registered: false,
      registering: false,
    }
  },
  props: {
  user: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  methods: {
    sendMessage() {
      this.registering = true;
      console.log(this.form);
      this.axios.post('http://localhost:9000/api/services/receive-message', this.form,
              {
                  headers: {
                    'Authorization': `Bearer ${this.user.accessToken}`
                  }
                })
          .then((res) => {
               //Perform Success Action
            console.log(res.data)
            this.registered = true;
            if (res.data.message === 'News added successfully!') {
              console.log('News added, close this modal')
              this.registered = true;
            }
           })
           .catch((error) => {
               // error.response.status Check status code
             console.log(error.message)
             this.error = 'Error encountered, please check your values!';
           }).finally(() => {
               //Perform action in always
              console.log('axios successfully invoked')
              this.registering = false
           });
    }
  }
}
</script>

<style scoped>

</style>
