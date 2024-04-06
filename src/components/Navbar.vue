<template>
  <div>
    <b-navbar toggleable="lg" type="dark">
      <div class="container mt-4">
        <b-navbar-brand class="m-0">
          <router-link to="/">
            <img src="../assets/images/restly.png" width="100px" />
          </router-link>
        </b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav class="ml-auto">
            <li class="nav-item">
              <router-link class="nav-link text-light" to="/">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link text-light" to="/foods"
                >Foods</router-link
              >
            </li>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <li class="nav-item">
              <router-link
                class="nav-link btn-success text-light pl-3 pr-3"
                to="/cart"
              >
                Cart
                <b-badge class="text-light" variant="warning">
                  <span class="badge-warning">{{
                    updateCart
                      ? updateCart.length
                      : amount.length
                  }}</span>
                </b-badge>
              </router-link>
            </li>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  data() {
    return {
      amount: [],
    };
  },
  props: ["updateCart"],
  methods: {
    setJumlah(data) {
      this.amount = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/cart")
      .then((response) => this.setJumlah(response.data))
      .catch((error) => console.log(error));
  },
};
</script>