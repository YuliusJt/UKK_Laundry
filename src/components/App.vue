<template>
  <div class="g-sidenav-show bg-gray-100">
    <div class="min-height-300 bg-primary position-absolute w-100"></div>
    <aside
      class="
        sidenav
        bg-white
        navbar navbar-vertical navbar-expand-xs
        border-0 border-radius-xl
        my-3
        fixed-start
        ms-4
      "
      id="sidenav-main"
    >
      <div class="sidenav-header">
        <i
          class="
            fas
            fa-times
            p-3
            cursor-pointer
            text-secondary
            opacity-5
            position-absolute
            end-0
            top-0
            d-none d-xl-none
          "
          aria-hidden="true"
          id="iconSidenav"
        ></i>
        <router-link class="navbar-brand m-0" to="/">
          <img src="src/assets/img/2150.jpg" class="navbar-brand-img h-100" />
          <span class="ms-1 font-weight-bold">Bloom Mandy</span>
        </router-link>
      </div>
      <hr class="horizontal dark mt-0" />
      <div class="collapse navbar-collapse w-auto" id="sidenav-collapse-main">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link
              class="nav-link active"
              to="/member"
              v-if="role !== 'owner'"
            >
              <div
                class="
                  icon icon-shape icon-sm
                  border-radius-md
                  text-center
                  me-2
                  d-flex
                  align-items-center
                  justify-content-center
                "
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-people-fill"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M7 14s-1 0-1-1 1-4 5-4 5 3 5 4-1 1-1 1H7zm4-6a3 3 0 1 0 0-6 3 3 0 0 0 0 6z"
                  />
                  <path
                    fill-rule="evenodd"
                    d="M5.216 14A2.238 2.238 0 0 1 5 13c0-1.355.68-2.75 1.936-3.72A6.325 6.325 0 0 0 5 9c-4 0-5 3-5 4s1 1 1 1h4.216z"
                  />
                  <path d="M4.5 8a2.5 2.5 0 1 0 0-5 2.5 2.5 0 0 0 0 5z" />
                </svg>
              </div>
              <span class="nav-link-text ms-1">Member</span>
            </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/outlet" v-if="role === 'admin'">
              <div
                class="
                  icon icon-shape icon-sm
                  border-radius-md
                  text-center
                  me-2
                  d-flex
                  align-items-center
                  justify-content-center
                "
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-bag-check-fill"
                  viewBox="0 0 16 16"
                >
                  <path
                    fill-rule="evenodd"
                    d="M10.5 3.5a2.5 2.5 0 0 0-5 0V4h5v-.5zm1 0V4H15v10a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V4h3.5v-.5a3.5 3.5 0 1 1 7 0zm-.646 5.354a.5.5 0 0 0-.708-.708L7.5 10.793 6.354 9.646a.5.5 0 1 0-.708.708l1.5 1.5a.5.5 0 0 0 .708 0l3-3z"
                  />
                </svg>
              </div>
              <span class="nav-link-text ms-1">Outlet</span>
            </router-link>
          </li>
          <li class="nav-item">
            <router-link
              class="nav-link"
              to="/transaksi"
              v-if="role === 'admin' || 'kasir' || 'owner'"
            >
              <div
                class="
                  icon icon-shape icon-sm
                  border-radius-md
                  text-center
                  me-2
                  d-flex
                  align-items-center
                  justify-content-center
                "
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-cart-check-fill"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M.5 1a.5.5 0 0 0 0 1h1.11l.401 1.607 1.498 7.985A.5.5 0 0 0 4 12h1a2 2 0 1 0 0 4 2 2 0 0 0 0-4h7a2 2 0 1 0 0 4 2 2 0 0 0 0-4h1a.5.5 0 0 0 .491-.408l1.5-8A.5.5 0 0 0 14.5 3H2.89l-.405-1.621A.5.5 0 0 0 2 1H.5zM6 14a1 1 0 1 1-2 0 1 1 0 0 1 2 0zm7 0a1 1 0 1 1-2 0 1 1 0 0 1 2 0zm-1.646-7.646-3 3a.5.5 0 0 1-.708 0l-1.5-1.5a.5.5 0 1 1 .708-.708L8 8.293l2.646-2.647a.5.5 0 0 1 .708.708z"
                  />
                </svg>
              </div>
              <span class="nav-link-text ms-1">Transaksi</span>
            </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/paket" v-if="role === 'admin'">
              <div
                class="
                  icon icon-shape icon-sm
                  border-radius-md
                  text-center
                  me-2
                  d-flex
                  align-items-center
                  justify-content-center
                "
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-box2-fill"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M3.75 0a1 1 0 0 0-.8.4L.1 4.2a.5.5 0 0 0-.1.3V15a1 1 0 0 0 1 1h14a1 1 0 0 0 1-1V4.5a.5.5 0 0 0-.1-.3L13.05.4a1 1 0 0 0-.8-.4h-8.5ZM15 4.667V5H1v-.333L1.5 4h6V1h1v3h6l.5.667Z"
                  />
                </svg>
              </div>
              <span class="nav-link-text ms-1">Paket</span>
            </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/user" v-if="role === 'admin'">
              <div
                class="
                  icon icon-shape icon-sm
                  border-radius-md
                  text-center
                  me-2
                  d-flex
                  align-items-center
                  justify-content-center
                "
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="20"
                  height="20"
                  fill="currentColor"
                  class="bi bi-person-circle"
                  viewBox="0 0 16 16"
                >
                  <path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0z" />
                  <path
                    fill-rule="evenodd"
                    d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8zm8-7a7 7 0 0 0-5.468 11.37C3.242 11.226 4.805 10 8 10s4.757 1.225 5.468 2.37A7 7 0 0 0 8 1z"
                  />
                </svg>
              </div>
              <span class="nav-link-text ms-1">User</span>
            </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/login.vue">
              <div
                class="nav-link"
                id="navbarDropdown"
                href="#"
                role="button"
                aria-expanded="false"
              >
                <a @click="Logout" href="#!">Logout</a>
              </div>
            </router-link>
          </li>
        </ul>
      </div>
    </aside>
    <main class="main-content position-relative border-radius-lg">
      <!-- Navbar -->
      <nav
        class="
          navbar navbar-main navbar-expand-lg
          px-0
          mx-4
          shadow-none
          border-radius-xl
        "
        id="navbarBlur"
        data-scroll="false"
      >
        <div class="container-fluid py-1 px-3">
          <nav aria-label="breadcrumb">
            <ol
              class="breadcrumb bg-transparent mb-0 pb-0 pt-1 px-0 me-sm-6 me-5"
            >
              <li class="breadcrumb-item text-sm">
                <a class="opacity-5 text-white" href="javascript:;">Pages</a>
              </li>
              <li
                class="breadcrumb-item text-sm text-white active"
                aria-current="page"
              >
                Dashboard
              </li>
            </ol>
            <h6 class="font-weight-bolder text-white mb-0">Dashboard</h6>
          </nav>
        </div>
      </nav>
      <router-view></router-view>
      <div id="layoutSidenav">
        <div id="layoutSidenav_content">
          <!-- MAIN -->

          <div card-body>
            <footer class="py-4 bg-light mt-auto">
              <div class="container-fluid px-4">
                <div
                  class="
                    d-flex
                    align-items-center
                    justify-content-between
                    small
                  "
                >
                  <div class="text-muted">
                    Copyright &copy; Your Website 2021
                  </div>
                  <div>
                    <a href="#">Privacy Policy</a>
                    &middot;
                    <a href="#">Terms &amp; Conditions</a>
                  </div>
                </div>
              </div>
            </footer>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
module.exports = {
  data: function () {
    return {
      username: "",
      role: "",
    };
  },
  methods: {
    getInfo: function () {
      let config = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };
      axios.get(base_url + "/login/check", config).then((response) => {
        if (response.data.success == true) {
          this.username = response.data.data.username;
          this.role = response.data.data.role;
        }
      });
    },
    Logout: function () {
      this.$cookies.remove("Authorization");
      this.componentName = "login";

      document.location.href = "/ukk_laundry";
    },
  },
  mounted() {
    this.getInfo();
  },
};
</script>