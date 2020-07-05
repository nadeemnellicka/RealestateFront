<template>
  <v-data-table
    :headers="headers"
    :items="contracts"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="black">
        <v-toolbar-title>Contracts</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn to="/re/contractCreate">New Contract</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="5" md="5">
                    <v-text-field v-model="editedItem.property_id" label="Property"></v-text-field>
                  </v-col>  
                   <v-col cols="12" sm="5" md="5">
                      <v-text-field v-model="editedItem.unit_id" label="Unit"></v-text-field>
                    </v-col>  


         <v-col cols="12" sm="5" md="5">
                      <v-date-picker
                        v-model="date"
                      ></v-date-picker>
                       </v-col> 




                  <v-col cols="12" sm="4" md="4">
                    <v-text-field height=""  v-model="editedItem.start_date" label="Start Date"></v-text-field>
                  </v-col> 
                  <v-col cols="12" sm="4" md="4">
                    <v-text-field v-model="editedItem.end_date" label="End Date"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="4" md="4">
                    <v-text-field v-model="editedItem.rent" label="Rent"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="4" md="4">
                    <v-text-field v-model="editedItem.deposit" label="Deposit"></v-text-field>
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
        { text: 'Property', value: 'property.name' },
        { text: 'Unit', value: 'unit.name' },
        { text: 'Start', value: 'start_date' },
        { text: 'End', value: 'end_date' },
        { text: 'Rent', value: 'rent' },
        { text: 'Deposit', value: 'deposit' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      contracts: [],
      property_type:[],
      properties:[],
      units:[],
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
      this.getProperties()
    },

    methods: {
    async  initialize () {
        try {
            const res = await this.$axios.get('realestate/contracts/list');
            this.contracts=res.data.data
          } catch (e) {
            console.error(e);
          }
      },
       async getProperties() {
          try {
            const res = await this.$axios.get('realestate/properties/list');
            this.properties=res.data.data
          } catch (e) {
            console.error(e);
          }
        }, 
        async getUnits(property_id) {
          try {
            const res = await this.$axios.get('realestate/units/list/'+property_id);
            this.properties=res.data.data
          } catch (e) {
            console.error(e);
          }
        },

      editItem (item) {
        this.editedIndex = this.contracts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        // this.$router.push({name: 're.contractCreate', params: { id: this.editedItem.id}})
        this.$router.push('/re/contractCreate');
        // this.dialog = true
      },

      deleteItem (item) {
        const index = this.contracts.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.contracts.splice(index, 1)
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
          Object.assign(this.contracts[this.editedIndex], this.editedItem)

        } else {
             try {
               const res =  this.$axios.post('realestate/tenants/create',this.editedItem);
               } catch (e) {
               console.error(e);
               }
          this.contracts.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>