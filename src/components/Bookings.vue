<template>
<section id="bookings" class="menu">
  <div class="container">

    <div class="section-title">
      <h2>This is the list of reservation requests | <span>Bookings</span></h2>
    </div>

    <div class="row menu-container">

      <div class="col-lg-6 menu-item filter-starters" style="padding: 4px;" v-for="(booking, i) in bookings" :key="i">
        <BookingItem :item="booking" />
      </div>

      <div v-if="bookings.length === 0">
        <p class="text-info text-center">
          Sorry! We currently have no reservations made.
        </p>
      </div>

    </div>

  </div>
</section>
</template>

<script>
import BookingItem from "@/components/BookingItem";
export default {
  name: "Bookings",
  components: {BookingItem},
  data() {
    return {
      bookings: [],
    }
  },
  created() {
    this.user = JSON.parse(localStorage.getItem('user'));
    setInterval(this.getBookings, 3000)
  },
  methods: {
    getBookings() {
      if (this.user == null) {
        return;
      }
      this.axios.get('http://localhost:9000/api/services/booking-listing',
          {
                  headers: {
                    'Authorization': `Bearer ${this.user.accessToken}`
                  }
                }
      ).then((response) => {
        try {
          if (response.data != null) {
            this.bookings = [];
            response.data.forEach((value) => {
              this.bookings.push(value);
            })
          }
        } catch (e) {
          console.log(e.message)
        }
      })
    }
  },
}
</script>

<style scoped>

</style>
