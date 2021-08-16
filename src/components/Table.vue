<template>
<section id="book-a-table" class="book-a-table">
      <div class="container">

        <div class="section-title">
          <h2>Book a <span>Table</span></h2>
          <p>We always appreciate when customers book before hand before they come to the restaurant, this eases planning.</p>
        </div>

        <form @submit.prevent="bookTable" method="post" role="form" class="php-email-form">
          <div class="row">
            <div class="col-lg-4 col-md-6 form-group">
              <input type="text" name="name" class="form-control" v-model="form.name" id="name" placeholder="Your Name" data-rule="minlen:4" data-msg="Please enter at least 4 chars">
              <div class="validate"></div>
            </div>
            <div class="col-lg-4 col-md-6 form-group mt-3 mt-md-0">
              <input type="email" class="form-control" name="email" v-model="form.email" id="email" placeholder="Your Email" data-rule="email" data-msg="Please enter a valid email">
              <div class="validate"></div>
            </div>
            <div class="col-lg-4 col-md-6 form-group mt-3 mt-md-0">
              <input type="text" class="form-control" name="phone" id="phone" v-model="form.phone" placeholder="Your Phone" data-rule="minlen:4" data-msg="Please enter at least 4 chars">
              <div class="validate"></div>
            </div>
            <div class="col-lg-4 col-md-6 form-group mt-3">
              <input type="text" name="date" class="form-control" id="date" v-model="form.date" placeholder="Date" data-rule="minlen:4" data-msg="Please enter at least 4 chars">
              <div class="validate"></div>
            </div>
            <div class="col-lg-4 col-md-6 form-group mt-3">
              <input type="text" class="form-control" name="time" id="time" v-model="form.time" placeholder="Time" data-rule="minlen:4" data-msg="Please enter at least 4 chars">
              <div class="validate"></div>
            </div>
            <div class="col-lg-4 col-md-6 form-group mt-3">
              <input type="number" class="form-control" name="people" id="people" v-model="form.people" placeholder="# of people" data-rule="minlen:1" data-msg="Please enter at least 1 chars">
              <div class="validate"></div>
            </div>
          </div>
          <div class="form-group mt-3">
            <textarea class="form-control" name="message" rows="5" v-model="form.message" placeholder="Message"></textarea>
            <div class="validate"></div>
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
                <div class="text-info text-center" v-if="registered">Your booking request was sent. We will call back or send an Email to confirm your reservation. Thank you!</div>
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
  name: "Table",
  data() {
    return {
      form: {
        name: '',
        email: '',
        phone: '',
        date: '',
        time: '',
        people: 0,
        message: ''
      },
      error: 'amd',
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
    bookTable() {
      this.registering = true;
      this.axios.post('http://localhost:9000/api/services/add-booking', this.form,
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
    },
  }
}
</script>

<style scoped>

</style>
