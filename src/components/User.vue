<template>
    <div>
        <main>
            <div class="container-fluid py-4">
      <div class="row">
        <div class="col-12">
          <h3>User</h3>
          <div class="card mb-4">
            <div class="card-header pb-0">
            </div>
            <div class="card-body px-0 pt-0 pb-2">
              <div class="table-responsive p-4">
                  <a class="btn btn-info btn-sm mb-0" v-b-modal.modal_user @click=Add()>Tambah</a>
                <table class="table align-items-center mb-0">
                                <tr>
                                    <td class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  ">ID User</td>
                                    <td class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  ">Nama</td>
                                    <td class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  ">Username</td>
                                    <td class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  ">role</td>
                                    <td class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  ">Outlet</td>
                                    <td class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  ">action</td>
                                </tr>
                                <tr v-for="ser in user" :key="ser">
                                    <td class="mb-0 text-sm">{{ ser.id }}</td>
                                    <td class="mb-0 text-sm">{{ ser.nama }}</td>
                                    <td class="mb-0 text-sm">{{ ser.username }}</td>
                                    <td class="mb-0 text-sm">{{ ser.role }}</td>
                                    <td class="mb-0 text-sm">{{ ser.outlet.nama_outlet }}</td>
                                    <td>
                                        <a v-b-modal.modal_user href="#" class="btn text-sm font-weight-bold text-xs btn-success" @click="Edit(ser)">Ubah</a>
                                        <a href="#" class="btn text-sm font-weight-bold text-xs btn-danger" @click="Drop(ser.id)">Hapus</a>
                                    </td>
                                </tr>
                            </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>

        <b-modal id="modal_user" ref="modal" title="Form User" size="md" @ok="Save">
            <form>  
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="nama" placeholder="masukkan name" id="inputNama" class="form-control" type="text"/>
                    <label for="inputNama">Nama</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="username" placeholder="masukkan username"  id="inputUsername" class="form-control" type="text"/>
                    <label for="inputUsername">Username</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="password" placeholder="masukkan password"  id="inputPassword" class="form-control" type="password"/>
                    <label for="inputUsername">Password</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    
                    <select id="inputRole" class="form-control" v-model="role">
                    <option value="">--Pilih Role--</option>
                    <option value="admin">Admin</option>
                    <option value="kasir">Kasir</option>
                    <option value="owner">Owner</option>
                    </select>
                    <label for="inputRole" >Role</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    
                    <b-form-select class="form-control" v-model="id_outlet" :options="data_outlet"></b-form-select>
                    <label for="id_outlet">Outlet</label>
                </div>
            </form>
        </b-modal>

    </div>
</template> 
<script>
module.exports = {
  data: function () {
    return {
      id_outlet: "",
      nama: "",
      username: "",
      password: "",
      role: "",
      action: "",
      message: "",
      key: "",
      user: [],
      data_outlet: [],
      fields: ["id", "nama", "username", "role", "outlet", "Aksi"],
    };
  },
  methods: {
    getData: function () {
      let conf = { headers: { Authorization: "Bearer " + this.key } };
      axios
        .get(base_url + "/user", conf)
        .then((response) => {
          if (response.data.success == true) {
            this.user = response.data.data.user;
          } else {
            this.componentName = "login";
            window.location = front_url;
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getOutletDropdown: function () {
      //ambil data outlet untuk dropdown
      let conf = { headers: { Authorization: "Bearer " + this.key } };
      axios.get(base_url + "/outlet", conf).then((response) => {
        let json_outlet = response.data.data.outlet;
        let list_outlet = [{ value: "", text: "-- Pilih Outlet --" }];
        json_outlet.forEach((element) => {
          list_outlet.push({
            value: element.id_outlet,
            text: element.nama_outlet,
          });
        });
        this.data_outlet = list_outlet;
      });
    },
    Add: function () {
      this.action = "insert";
      this.id_outlet = "";
      this.id = "";
      this.nama = "";
      this.username = "";
      this.password = "";
      this.role = "";
    },
    Edit: function (item) {
      this.action = "update";
      this.id_outlet = item.id_outlet;
      this.id = item.id;
      this.nama = item.nama;
      this.username = item.username;
      this.role = item.role;
    },
    Save: function () {
      let conf = { headers: { Authorization: "Bearer " + this.key } };
      this.$bvModal.hide("modal-user");
      let form = {
        id_outlet: this.id_outlet,
        nama: this.nama,
        username: this.username,
        password: this.password,
        role: this.role,
      };
      if (this.action === "insert") {
        axios
          .post(base_url + "/user", form, conf)
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
          .put(base_url + "/user/" + this.id, form, conf)
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
          .delete(base_url + "/user/" + id, conf)
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
    this.getOutletDropdown();
  },
};
</script>