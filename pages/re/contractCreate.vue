<template>
  <v-form v-model="valid">
    <v-container>
      <v-row>
       
        
        <v-col  cols="12"  md="6" >
          <v-select v-model="editedItem.property_id" item-text="building_no" @change="getUnits(1)" item-value="id" :items="properties" label="Property" ></v-select>
        </v-col>
        <v-col  cols="12"  md="6" >
          <v-select v-model="editedItem.unit_id" item-text="name" item-value="id" :items="units" label="Unit" ></v-select>
        </v-col>
         <v-col  cols="12"  md="4" >
          <v-select v-model="editedItem.tenant_id" item-text="name" item-value="id" :items="tenants" label="Tenant" ></v-select>
        </v-col>
        <v-col  cols="12"  md="4" >
          <v-text-field prepend-icon="mdi-calendar-range" v-model="editedItem.start_date" label="Start(yyyy/mm/dd)"></v-text-field>
        </v-col>
        <v-col  cols="12"  md="4" >
          <v-text-field prepend-icon="mdi-calendar-range" v-model="editedItem.end_date" label="End(yyyy/mm/dd)"></v-text-field>
        </v-col>
        <v-col  cols="12"  md="6" >
          <v-text-field prepend-icon="mdi-cash-usd" v-model="editedItem.rent" label="Rent"></v-text-field>
        </v-col>
        <v-col  cols="12"  md="6" >
          <v-text-field prepend-icon="mdi-cash-usd" v-model="editedItem.deposit" label="Depost"></v-text-field>
        </v-col>
        <v-btn color="blue darken-1" text @click="save">Save</v-btn>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
  export default {
    data: () => ({
      editedItem:{},
      properties:[],
      units:[],
    }),
    created () {
      this.initialize()
      this.getProperties()
      this.getTenants()
    },
     methods: {
    async  initialize () {
        try {
            const res = await this.$axios.get('realestate/properties/list');
            this.desserts=res.data.data
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
       async getTenants() {
          try {
            const res = await this.$axios.get('realestate/tenants/list');
            this.tenants=res.data.data
          } catch (e) {
            console.error(e);
          }
      },  
      async getUnits(property_id) {
          try {
            const res = await this.$axios.get('realestate/units/list');
            this.units=res.data.data
          } catch (e) {
            console.error(e);
          }
      }, 

      deleteItem (item) {
        const index = this.desserts.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1)
      },

      save () {
        // if (this.editedIndex > -1) {
        //       try {
        //        const res =  this.$axios.post('realestate/contact/create',this.editedItem);
        //        } catch (e) {
        //        console.error(e);
        //        }
        //   Object.assign(this.desserts[this.editedIndex], this.editedItem)

        // } else {
             try {
              debugger
               const res =  this.$axios.post('realestate/contracts/create', this.editedItem);
               } catch (e) {
               console.error(e);
               }
      },

    },
  }
</script>