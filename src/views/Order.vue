<template>
  <div class="order">
    <Navbar />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center p-2">
          <img src="../assets/images/success-order.png" width="300" />
          <h2 class="mt-5"><strong>Thankyou for Order</strong></h2>
          <h4>
            Please Confirm Your Order to Cashier <br />
            Enjoy Your Food
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "Order",
  components: {
    Navbar,
  },
};
</script>