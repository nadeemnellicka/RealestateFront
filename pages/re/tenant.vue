<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="black">
        <v-toolbar-title>Tenants</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >New Item</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="5" md="5">
                    <v-text-field v-model="editedItem.name" label="Name"></v-text-field>
                  </v-col>  
                   <v-col cols="12" sm="5" md="5">
                      <v-text-field v-model="editedItem.national_id" label="Qatar id"></v-text-field>
                    </v-col>  
                  <v-col cols="12" sm="4" md="4">
                    <v-text-field height=""  v-model="editedItem.phone" label="Phone No."></v-text-field>
                  </v-col> 
                  <v-col cols="12" sm="4" md="4">
                    <v-text-field v-model="editedItem.nationality" label="Nationality"></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
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
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      headers: [
        { text: 'Name', value: 'name' },
        { text: 'Phone', value: 'phone' },
        { text: 'Qatar Id', value: 'national_id' },
        { text: 'Nationality', value: 'nationality' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      desserts: [],
      property_type:[],
      editedIndex: -1,
      editedItem: {
        type: '',
        name: '',
      },
      defaultItem: {
        type: '',
        name: '',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
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
    async  initialize () {
        try {
            const res = await this.$axios.get('realestate/tenants/list');
            this.desserts=res.data.data
          } catch (e) {
            console.error(e);
          }
      },
      editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.desserts.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1)
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
        if (this.editedIndex > -1) {
              try {
               const res =  this.$axios.post('realestate/tenants/create',this.editedItem);
               } catch (e) {
               console.error(e);
               }
          Object.assign(this.desserts[this.editedIndex], this.editedItem)

        } else {
             try {
               const res =  this.$axios.post('realestate/tenants/create',this.editedItem);
               } catch (e) {
               console.error(e);
               }
          this.desserts.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>