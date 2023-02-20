<template>
  <div>
    <div class="content-wrapper">
      <div class="row">
        <div class="col-lg-12 grid-margin stretch-card">
          <h3>Data Paket</h3>
          <div class="card">
            <div class="card-body">
              <p class="card-description float-right">
                <b-button
                  size="sm"
                  variant="success"
                  v-b-modal.modal-paket
                  v-on:click="Add()"
                  ><i class="mdi mdi-plus btn-icon-prepend"></i>
                  Tambah</b-button
                >
              </p>
              <table class="table align-items-center mb-0">
                <tr>
                  <td
                    class="
                      text-uppercase text-secondary text-xxs
                      font-weight-bolder
                      opacity-7
                    "
                  >
                    ID PAKET
                  </td>
                  <td
                    class="
                      text-uppercase text-secondary text-xxs
                      font-weight-bolder
                      opacity-7
                    "
                  >
                    JENIS
                  </td>
                  <td
                    class="
                      text-uppercase text-secondary text-xxs
                      font-weight-bolder
                      opacity-7
                    "
                  >
                    HARGA
                  </td>
                  <td
                    class="
                      text-uppercase text-secondary text-xxs
                      font-weight-bolder
                      opacity-7
                    "
                  >
                    AKSI
                  </td>
                </tr>
                <tr v-for="pak in paket" :key="pak">
                  <td class="mb-0 text-sm">{{ pak.id_paket }}</td>
                  <td class="mb-0 text-sm">{{ pak.jenis }}</td>
                  <td class="mb-0 text-sm">{{ pak.harga }}</td>
                  <td>
                    <a
                      v-b-modal.modal-paket
                      href="#"
                      class="btn text-secondary font-weight-bold text-xs"
                      data-original-title="Edit paket"
                      @click="Edit(pak)"
                      >Ubah</a
                    >
                    <a
                      href="#"
                      class="btn text-secondary font-weight-bold text-xs"
                      data-original-title="Edit paket"
                      @click="Drop(pak.id_paket)"
                      >Hapus</a
                    >
                  </td>
                </tr>
                <b-toast id="message" title="Message">
                  <strong class="text-success">{{ message }}</strong>
                </b-toast>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>

    <b-modal
      id="modal-paket"
      ref="modal"
      title="Form Paket"
      size="sm"
      @ok="Save"
    >
      <form>
        <div class="form-group">
          <label for="jenis" class="col-form-label">Jenis</label>
          <select class="form-control" v-model="jenis">
            <option value="kiloan">Kiloan</option>
            <option value="selimut">Selimut</option>
            <option value="kaos">Kaos</option>
            <option value="bed_cover">Bed Cover</option>
          </select>
        </div>
        <div class="form-group">
          <label for="nama" class="col-form-label">Harga</label>
          <input
            type="number"
            name="harga"
            class="form-control"
            id="harga"
            placeholder="Harga (RP)"
            v-model="harga"
          />
        </div>
      </form>
    </b-modal>
  </div>
</template>

<script>
module.exports = {
  data: function () {
    return {
      search: "",
      id_paket: "",
      jenis: "",
      harga: "",
      action: "",
      message: "",
      key: "",
      paket: [],
      fields: ["id_paket", "jenis", "harga", "Aksi"],
    };
  },

  methods: {
    getData: function () {
      let conf = { headers: { Authorization: "Bearer " + this.key } };
      axios
        .get(base_url + "/paket", conf)
        .then((response) => {
          if (response.data.success == true) {
            this.paket = response.data.data.paket;
          } else {
            this.componentName = "login";
            window.location = front_url;
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },

    Add: function () {
      this.action = "insert";
      this.id_paket = "";
      this.jenis = "";
      this.harga = "";
    },

    Edit: function (item) {
      this.action = "update";
      this.id_paket = item.id_paket;
      this.jenis = item.jenis;
      this.harga = item.harga;
    },

    Save: function () {
      let conf = { headers: { Authorization: "Bearer " + this.key } };
      this.$bvModal.hide("modal-paket");
      let form = {
        id_paket: this.id_paket,
        jenis: this.jenis,
        harga: this.harga,
      };

      if (this.action === "insert") {
        axios
          .post(base_url + "/paket", form, conf)
          .then((response) => {
            this.getData();
            this.message = response.data.message;
            this.$bvToast.show("message");
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        axios
          .put(base_url + "/paket/" + this.id_paket, form, conf)
          .then((response) => {
            this.getData();
            this.message = response.data.message;
            this.$bvToast.show("message");
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },

    Drop: function (id) {
      let conf = { headers: { Authorization: "Bearer " + this.key } };
      if (confirm("Apakah anda yakin ingin menghapus data ini?")) {
        axios
          .delete(base_url + "/paket/" + id, conf)
          .then((response) => {
            if (response.data.success == true) {
              this.getData();
            }
            this.message = response.data.message;
            this.$bvToast.show("message");
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
  },
  mounted() {
    this.key = this.$cookies.get("Authorization");
    this.getData();
  },
};
</script>