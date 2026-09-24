<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-pastel">
      <div class="container">

        <router-link
          :to="{ name: 'home' }"
          class="navbar-brand"
        >
          HOME
        </router-link>

        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>

        <div
          class="collapse navbar-collapse"
          id="navbarSupportedContent"
        >

          <ul class="navbar-nav me-auto mb-2 mb-lg-0">

            <li class="nav-item">
              <router-link
                to="/products"
                class="nav-link"
              >
                PRODUCTS
              </router-link>
            </li>

          </ul>

        </div>
      </div>
    </nav>

    <!-- render router view -->
    <router-view />

  </div>
</template>

<style scoped>
.navbar-pastel {
  background-color: #d8c4e8;
}

.navbar-brand,
.nav-link {
  color: #4f405f !important;
}

.navbar-brand {
  font-weight: 600;
}

.nav-link {
  font-weight: 500;
}

.nav-link:hover,
.navbar-brand:hover {
  color: #2f2638 !important;
}
</style>