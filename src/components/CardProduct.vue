<template>
  <div>
    <!-- Desktop -->
    <div
      class="card card-product d-none d-lg-block mt-4 mb-4"
      style="max-width: 330px"
    >
      <div class="row no-gutters">
        <div class="col-sm-3">
          <img
            :src="'../assets/images/' + product.image"
            width="100%"
            class="img-card-dekstop"
            alt="image food"
          />
        </div>
        <div class="col-sm-9">
          <div class="card-body pb-0 ml-4">
            <h6 class="card-title pt-3 mb-1">{{ product.name }}</h6>
            <div class="row">
              <div class="col-6">
                <p class="card-text text-warning">{{ product.code }}</p>
              </div>
              <div class="col-6 pl-0">
                <p class="card-text text-danger">
                  <strong>Rp. {{ product.price | numFormat("0,0")  }}</strong>
                </p>
              </div>
            </div>
            <router-link
              class="card-text text-light btn-card"
              :to="'/foods/' + product.id"
            >
              Order Now
            </router-link>
          </div>
        </div>
        <router-link
          class="btn btn-success btn-card-cart"
          :to="'/foods/' + product.id"
          ><b-icon-cart font-scale="1" shift-v="3"></b-icon-cart
        ></router-link>
      </div>
    </div>

    <!-- Mobile -->
    <div
      class="card shadow card-product d-md-block d-lg-none mt-4 mb-4 mx-auto"
      style="max-height: 320px; max-width: 300px"
    >
      <b-img
        :src="'assets/images/' + product.image"
        class="img-card-mobile"
        alt="image food"
        center
      />
      <div class="card-body">
        <h6 class="card-title">{{ product.name }}</h6>
        <div class="row">
          <div class="col-6">
            <p class="card-text text-warning">{{ product.code }}</p>
          </div>
          <div class="col-6 pl-0">
            <p class="card-text text-right text-danger">
              <strong>Rp. {{ product.price | numFormat("0,0") }}</strong>
            </p>
          </div>
        </div>
        <div class="row mt-4">
          <div class="col-6">
            <router-link
              class="card-text text-light btn-card-mobile"
              :to="'/foods/' + product.id"
            >
              Order Now
            </router-link>
          </div>
          <div class="col-6 pl-0">
            <router-link
              class="btn btn-success btn-card-cart"
              :to="'/foods/' + product.id"
              ><b-icon-cart font-scale="1" shift-v="3"></b-icon-cart
            ></router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProduct",
  props: ["product"],
};
</script>