<template>
  <div class="hero">
    <!-- Desktop -->
    <div class="d-none d-md-block">
      <div class="row mt-5">
        <div class="col-md-5">
          <div class="d-flex">
            <div class="justify-content-center align-self-center">
              <p>EASY WAY TO ORDERS YOUR FOOD</p>
              <h1>
                <strong>
                  Order Tasty & <br />Fresh Food <br />
                  <span class="text-danger">anytime!</span>
                </strong>
              </h1>
              <p class="pt-2 pb-2">
                Just confirm your order and enjoy our<br />choose delicious
                food.
              </p>
              <router-link
                class="btn btn-lg btn-danger mr-4"
                :to="'/foods/' + product.id"
              >
                Order Now
              </router-link>
              <router-link class="btn-warning ml-4" to="/foods"
                >See Menu
              </router-link>
            </div>
          </div>
        </div>
        <div class="col-md-7">
          <img
            :src="'../assets/images/' + product.gambar"
            width="100%"
            alt="images hero"
          />
        </div>
      </div>
    </div>

    <!-- Mobile -->
    <div class="d-sm-block d-md-none">
      <div class="row mt-4">
        <div class="col-md-6 mb-3">
          <img
            :src="'../assets/images/' + product.gambar"
            width="100%"
            alt="images hero"
          />
        </div>
        <div class="col-md-6">
          <div class="d-flex h-100 p-2">
            <div class="justify-content-center align-self-center">
              <p>EASY WAY TO ORDERS YOUR FOOD</p>
              <h1>
                <strong>
                  Order Tasty & <br />Fresh Food <br />
                  <span class="text-danger">anytime!</span>
                </strong>
              </h1>
              <p class="pt-2 pb-2">
                Just confirm your order and enjoy our<br />choose delicious
                food.
              </p>
              <router-link
                class="btn btn-lg btn-danger"
                :to="'/foods/' + product.id"
              >
                Order Now
              </router-link>
              <router-link class="btn-warning ml-4" to="/foods"
                >See Menu
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "Hero",
  data() {
    return {
      product: {},
    };
  },
  methods: {
    setProducts(data) {
      this.product = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/hero/9")
      .then((response) => this.setProducts(response.data))
      .catch((error) => console.log(error));
  },
};
</script>