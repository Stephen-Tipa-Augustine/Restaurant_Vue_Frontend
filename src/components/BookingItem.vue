<template>
  <div>
    <div class="card">
    <div class="card-header">
      <div class="menu-content">
        <a href="#">Booked by {{item.name}}</a><span>For {{item.people}} Person(s)</span>
      </div>
    </div>
    <div class="card-body">
      <div class="menu-ingredients">
        <p class="text-body">
          {{item.message}}
        </p>
        <p class="text-secondary">
          Scheduled for date: {{item.date}}, time: {{item.time}}
        </p>
      </div>
    </div>
    <div class="card-footer">
      <div class="menu-ingredients">
        <p class="text-info">
          Phone: {{item.phone}} or email: {{item.email}}
        </p>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  name: "BookingItem",
  props: {
    item: {
      type: Object,
      default() {
        return {}
      }
    }
  },
}
</script>

<style scoped>

</style>
