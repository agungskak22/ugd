<template>
<div style="padding:10px">
  <v-data-table
    :headers="headers"
    :items="mahasiswa"
    sort-by="NomorMahasiswa"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Tabel Mahasiswa</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <div class="flex-grow-1"></div>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">Mahasiswa Baru</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.name" label="Nama Mahasiswa"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.NomorMahasiswa" label="Nomor Pokok Mahasiswa"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Umur" label="Umur (th)"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.NomorHandphone" label="Nomor Handphone"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.indexKomulatif" label="IPK"></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <div class="flex-grow-1"></div>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.action="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
  </div>
</template>
<script>
  export default {
    data: () => ({
      dialog: false,
      headers: [
        {
          text: 'Nama Mahasiswa',
          align: 'left',
          sortable: false,
          value: 'name',
        },
        { text: 'Nomor Mahasiswa', value: 'NomorMahasiswa' },
        { text: 'Umur', value: 'Umur' },
        { text: 'Nomor Handphone', value: 'NomorHandphone' },
        { text: 'IPK', value: 'indexKomulatif' },
        { text: 'Actions', value: 'action', sortable: false },
      ],
      mahasiswa: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        NomorMahasiswa: '',
        Umur: '',
        NomorHandphone: '',
        indexKomulatif: '',
      },
      defaultItem: {
        name: '',
        NomorMahasiswa: '',
        Umur: '',
        NomorHandphone: '',
        indexKomulatif: '',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'Mahasiswa Baru' : 'Edit Mahasiswa'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.mahasiswa = [
          {
            name: 'Agung Prio Rismawan',
            NomorMahasiswa: 160709001,
            Umur: 21,
            NomorHandphone: '081254026142',
            indexKomulatif: 3.8
          },
          {
            name: 'Rius Sabarno',
            NomorMahasiswa: 160709002,
            Umur: 22,
            NomorHandphone: '081245658701',
            indexKomulatif: 4.0
          },{
            name: 'Rio Gunawan',
            NomorMahasiswa: 160709003,
            Umur: 23,
            NomorHandphone: '081254036124',
            indexKomulatif: 3.5
          },{
            name: 'Yafet trakan Tangkuman',
            NomorMahasiswa: 160709004,
            Umur: 22,
            NomorHandphone: '081243554321',
            indexKomulatif: 4.0
          },{
            name: 'RObertus Yudho S',
            NomorMahasiswa: 160709005,
            Umur: 21,
            NomorHandphone: '082354246612',
            indexKomulatif: 3.6
          },{
            name: 'Devina Arnindyasari',
            NomorMahasiswa: 160709006,
            Umur: 20,
            NomorHandphone: '082354246612',
            indexKomulatif: 4.0
          },{
            name: 'Satria nusa paradilaga',
            NomorMahasiswa: 160709007,
            Umur: 21,
            NomorHandphone: '081265431122',
            indexKomulatif: 4.0
          },
        ]
      },

      editItem (item) {
        this.editedIndex = this.mahasiswa.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.mahasiswa.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.mahasiswa.splice(index, 1)
      },

      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.mahasiswa[this.editedIndex], this.editedItem)
        } else {
          this.mahasiswa.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>