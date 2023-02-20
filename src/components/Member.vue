<template>
  <div>
    <main>
      <div class="container-fluid py-4">
        <h3 class="row">Data Member</h3>
        <div class="card mb-4">
          <div class="card-header pd-0">
            <a class="btn btn-warning" v-b-modal.modal_member @click="Add()"
              >Tambah Member</a
            >
            <table class="table align-items-center mb-0">
              <tr>
                <td
                  class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  "
                >
                  ID MEMBER
                </td>
                <td
                  class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  "
                >
                  NAMA
                </td>
                <td
                  class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  "
                >
                  JK
                </td>
                <td
                  class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  "
                >
                  TLP
                </td>
                <td
                  class="
                    text-uppercase text-secondary text-xxs
                    font-weight-bolder
                    opacity-7
                  "
                >
                  ALAMAT
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
              <tr v-for="mem in member" :key="mem">
                <td class="mb-0 text-sm">{{ mem.id_member }}</td>
                <td class="mb-0 text-sm">{{ mem.nama }}</td>
                <td class="mb-0 text-sm">{{ mem.jenis_kelamin }}</td>
                <td class="mb-0 text-sm">{{ mem.tlp }}</td>
                <td class="mb-0 text-sm">{{ mem.alamat }}</td>
                <td>
                  <a
                    v-b-modal.modal_member
                    href="javascript:;"
                    class="btn text-secondary font-weight-bold text-xs"
                    data-toggle="tooltip"
                    data-original-title="Edit user"
                    @click="Edit(mem)"
                    >Ubah</a
                  >
                  <a
                    href="javascript:;"
                    class="btn text-secondary font-weight-bold text-xs"
                    data-toggle="tooltip"
                    data-original-title="Edit user"
                    @click="Delete(mem.id_member)"
                    >Hapus</a
                  >
                </td>
              </tr>
            </table>
          </div>
        </div>
      </div>
    </main>

    <b-modal
      id="modal_member"
      ref="modal"
      title="Form Member"
      size="md"
      @ok="Save"
    >
      <form>
        <div class="form-floating mb-3 mb-md-0">
          <input
            v-model="nama"
            placeholder="Enter your first name"
            v-modal="nama"
            id="inputNama"
            class="form-control"
            type="text"
          />
          <label for="inputNama">Nama</label>
        </div>
        <div class="form-floating mb-3 mb-md-0">
          <input
            v-model="tlp"
            placeholder="Enter your first name"
            v-modal="tlp"
            id="inputTlp"
            class="form-control"
            type="text"
          />
          <label for="inputTlp">Telepon</label>
        </div>
        <div class="form-floating mb-3 mb-md-0">
          <select v-model="jk" class="form-control">
            <option value="l">Laki-Laki</option>
            <option value="p">Perempuan</option>
          </select>

          <label for="inputJk">Jenis Kelamin</label>
        </div>
        <div class="form-floating mb-3 mb-md-0">
          <input
            v-model="alamat"
            placeholder="Enter your first name"
            v-modal="alamat"
            id="inputAlamat"
            class="form-control"
            type="text"
          />
          <label for="inputAlamat">Alamat</label>
        </div>
      </form>
    </b-modal>
  </div>
</template>
<script>
module.exports = {
  data: function () {
    return {
      id_member: "",
      nama: "",
      jk: "",
      tlp: "",
      alamat: "",
      member: [],
      action: "",
    };
  },
  methods: {
    getData: function () {
      let config = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };

      axios.get(base_url + "/member", config).then((response) => {
        console.log(response);
        if (response.data.success == true) {
          this.member = response.data.data.member;
        }
      });
    },
    Add: function () {
      this.action = "insert";
      this.id_member = "";
      this.nama = "";
      this.jk = "";
      this.tlp = "";
      this.alamat = "";
    },
    Edit: function (item) {
      this.action = "update";
      this.id_member = item.id_member;
      this.nama = item.nama;
      this.jk = item.jenis_kelamin;
      this.tlp = item.tlp;
      this.alamat = item.alamat;
    },
    Save: function () {
      let config = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };

      let form = {
        nama: this.nama,
        alamat: this.alamat,
        jenis_kelamin: this.jk,
        tlp: this.tlp,
      };

      //logika method post/get (insert /update)
      if (this.action == "insert") {
        axios.post(base_url + "/member", form, config).then((response) => {
          alert(response.data.message);
        });
      } else {
        //update
        axios
          .put(base_url + "/member/" + this.id_member, form, config)
          .then((response) => {
            alert(response.data.message);
          });
      }

      this.getData();
    },
    Delete: function (id) {
      if (confirm("Apakah anda yakin menghapus data member ini?")) {
        let config = {
          headers: {
            Authorization: "Bearer " + this.$cookies.get("Authorization"),
          },
        };

        axios.delete(base_url + "/member/" + id, config).then((response) => {
          alert(response.data.message);
        });

        this.getData();
      }

      // Swal.fire({
      //     title: 'Error!',
      //     text: 'Do you want to continue',
      //     icon: 'error',
      //     confirmButtonText: 'Cool'
      // })
    },
  },
  mounted() {
    this.getData();
  },
};
</script>