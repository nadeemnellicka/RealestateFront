<template>
  <v-data-table
    :headers="headers"
    :items="rentalTracker"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="black">
        <v-toolbar-title>Rental Tracker</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>
            <v-card-text>
              <v-container>

                 <v-card-text>
      <div>{{editedItem.contract.unit.name}}</div>
      <p class="display-1 text--primary">
        {{editedItem.contract.tenant.name}}
      </p>
      <p>{{editedItem.contract.property.name}}</p>
      <p>{{editedItem.contract.unit.name}}</p>
    </v-card-text>


                <v-row>
                  You are collecting the rent of {{editedItem.contract.unit.name}} of {{editedItem.contract.property.name}} for the month of {{editedItem.start_date}} of {{editedItem.amount}}
                  <v-col cols="12" sm="6" md="6">
                       <h1>Rent Amount:{{editedItem.amount}}</h1>  
                  </v-col> 
                  <v-col cols="12" sm="6" md="6">
                        <v-text-field label="Balance Amount"  v-model="editedItem.amount" ></v-text-field>
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
        { text: 'Property', value: 'contract.property.name' },
        { text: 'Unit', value: 'contract.unit.name' },
        { text: 'Tenant', value: 'contract.tenant.name' },
        { text: 'amount', value: 'amount' },
        { text: 'start_date', value: 'start_date' },
        { text: 'end_date', value: 'end_date' },
        { text: 'status', value: 'status' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      rentalTracker: [],
      properties:[],
      editedIndex: -1,
      editedItem: {
        contract: {
          property:{
            name:"",
          },
          unit:{
            name:"",
          },
          tenant:{
            name:"",
          },
        },
        name: '',
      },
      defaultItem: {
        type: '',
        name: '',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Make Collection'
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
            const res = await this.$axios.get('realestate/tracker/list');
            this.rentalTracker=res.data.data
          } catch (e) {
            console.error(e);
          }
      },
      editItem (item) {
        this.editedIndex = this.rentalTracker.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
        debugger
      },

      deleteItem (item) {
        const index = this.rentalTracker.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.rentalTracker.splice(index, 1)
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
               const res =  this.$axios.put('realestate/units/update',this.editedItem);
               } catch (e) {
               console.error(e);
               }
          Object.assign(this.rentalTracker[this.editedIndex], this.editedItem)

        } else {
             try {
               const res =  this.$axios.post('realestate/units/create',this.editedItem);
               } catch (e) {
               console.error(e);
               }
          this.rentalTracker.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>