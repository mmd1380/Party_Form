<template>
  <v-data-table
      :headers="headers"
      :items="Parties"
      class="elevation-1"
      hide-default-footer
  >
    <template v-slot:top>
      <v-toolbar
          flat
      >
        <v-dialog
            v-model="dialog"
            max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
                color="primary"
                dark
                class="mb-2"
                v-bind="attrs"
                v-on="on"
            >
              آدرس جدید
            </v-btn>
          </template>
          <v-card>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                      cols="12"
                  >

                    <v-autocomplete
                        v-model="editedItem.AddressType"
                        label="AddressType"
                        :items="editedItem.AddressType"
                        required
                        clearable
                        dense
                        reverse
                        filled

                    ></v-autocomplete>


                  </v-col>
                  <v-col
                      cols="12"

                  >
                    <v-text-field
                        v-model="editedItem.PostCode"
                        label="PostCode"
                    ></v-text-field>
                  </v-col>
                  <v-col
                      cols="12"

                  >

                    <v-autocomplete
                        v-model="editedItem.AccommodationCity"
                        label="AccommodationCity "
                        :items="editedItem.AccommodationCities"
                        required
                        clearable
                        dense
                        reverse
                        filled

                    ></v-autocomplete>

                  </v-col>
                  <v-col
                      cols="12"
                  >
                    <v-text-field
                        v-model="editedItem.AccommodationAddress"
                        label="AccommodationAddress"
                    ></v-text-field>
                  </v-col>
                  <v-col
                      cols="12"
                  >
                    <v-text-field
                        v-model="editedItem.AccommodationTelephone"
                        label="AccommodationTelephone"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                  color="blue darken-1"
                  text
                  @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                  color="blue darken-1"
                  text
                  @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">از پاک کردن این آدرس مطمئن هستید؟</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">خیر</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">بله</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }" class="d-flex justify-space-around">
      <v-icon
          small
          class="mr-1"
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
  </v-data-table>
</template>

<script>
export default {
  AddressType: "AddressCard",
  data: () => ({

    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: 'نوع آدرس',
        align: 'start',
        sortable: false,
        value: 'AddressType',
      },
      { text: 'کد پستی', value: 'PostCode' , sortable: false},
      { text: 'شهر', value: 'AccommodationCity' , sortable: false},
      { text: 'آدرس', value: 'AccommodationAddress' , sortable: false},
      { text: 'تلفن', value: 'AccommodationTelephone', sortable: false },
      { text: ' ', value: 'actions', sortable: false },
    ],
    Parties: [],
    editedIndex: -1,
    editedItem: {
      AddressType: '',
      AddressTypes: ["محل کار", "محل زندگی"],
      PostCode: '',
      AccommodationCity: null,
      AccommodationCities: ["تهران", "تبریز", "شیراز", "همدان"],
      AccommodationAddress: '',
      AccommodationTelephone: '',
    },
    defaultItem: {
      AddressType: '',
      PostCode: 0,
      AccommodationCity: 0,
      AccommodationAddress: 0,
      AccommodationTelephone: 0,
    },
  }),



  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      this.Parties = [
        {
          AddressType: 'محل زندگی',
          PostCode: 113121155,
          AccommodationCity: 'تهران',
          AccommodationAddress: 'خیابان ولیعصر - خیابان انقلاب - کوچه پشن ',
          AccommodationTelephone: '021-88848414',
        },
        {
          AddressType: 'محل کار',
          PostCode: 113121155,
          AccommodationCity: 'تهران',
          AccommodationAddress: 'خیابان ولیعصر - خیابان انقلاب - کوچه پشن ',
          AccommodationTelephone: '021-88848414',
        },
      ]
    },

    editItem (item) {
      this.editedIndex = this.Parties.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.editedIndex = this.Parties.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm () {
      this.Parties.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.Parties[this.editedIndex], this.editedItem)
      } else {
        this.Parties.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>
