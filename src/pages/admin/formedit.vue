<template>
  <q-page padding>
  <div class="row q-mb-md col-gutter-md">
    <div class="col-md-12 col-xs-12 col-lg-12">
      <div class="row">
        <div class="col-auto">
          <div class="left blue"></div>
          </div>
        <div class="col">
          <q-banner inline-actions class="text-blue-grey-14">
            <div class="text-h6">Edit Raport</div>
            <div>Edit Data </div>
          </q-banner>
        </div>
      </div>
    </div>
  </div>
  <q-card flat>
    <q-card-section class="row =">
    <q-form
      @submit="onSubmit()"
      class="q-col-gutter-md q-col-lg-6 col-md-6 col-xs-12"
      >
      <q-input
          filled
          v-model="form.judulFilm"
          label="Nama "
          :rules="[ val => val && val.length > 0 || 'Mohon Isi Nama']"
      />
      <q-input
          filled
          type="number"
          v-model="form.harga"
          label="Nilai"
          :rules="[ val => val && val.length > 0 || 'Mohon Isi Nilai']"
      />
      <q-input
          filled
          v-model="form.tahun"
          label="Kelas"
          :rules="[ val => val && val.length > 0 || 'Mohon Isi Kelas']"
      />
      <q-input
      v-model="form.deskripsi"
      filled
      label="Mata Pelajaran"
    />
      <div>
          <q-btn label="Submit" type="submit" color="primary"/>
          <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
      </div>
     </q-form>
    </q-card-section>
  </q-card>
  </q-page>
</template>
<script>
export default {
  data () {
    return {
      form: {
        judulFilm: null,
        harga: null,
        tahun: null,
        genre: null,
        rating: 0,
        deskripsi: null
      },
      optionGenre: [
        'Action',
        'Adventure',
        'Comedy',
        'Drama',
        'Fantasy'
      ],
      image: null
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get(`movie/getbyid/${this.$route.params.id}`)
        .then(res => {
          if (res.data.sukses) {
            this.form = res.data.data
          } else {
            this.$router.push({ name: 'dataDVD' })
          }
        })
    },
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.put(`movie/edit/${this.$route.params.id}`, formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.$router.push({ name: 'dataDVD' })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    }
  }
}
</script>
<style scoped>
.left {
  width: 3px;
  height: 100%;
  background-color: rgb(3, 7, 63);
}
</style>
