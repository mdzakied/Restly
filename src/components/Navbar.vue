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
                to="/keranjang"
              >
                Cart Food
                <b-icon-bag class="ml-1" shift-v="2"></b-icon-bag>
                <b-badge class="ml-1" variant="danger">
                  <span class="badge-danger">{{
                    updateKeranjang
                      ? updateKeranjang.length
                      : jumlah_pesanans.length
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
      jumlah_pesanans: [],
    };
  },
  props: ["updateKeranjang"],
  methods: {
    setJumlah(data) {
      this.jumlah_pesanans = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setJumlah(response.data))
      .catch((error) => console.log(error));
  },
};
</script>