<template>
  <v-simple-table>
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">Name</th>
          <th class="text-left">Property Type</th>
          <th class="text-left">Rent</th>
          <th class="text-left">Unit Count</th>
          <th class="text-left">Budgetted Rent</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in propertyReport" :key="item.name">
          <td>{{ item.name }}</td>
          <td>{{ item.property_type }}</td>
          <td>{{ item.rent }}</td>
          <td><nuxt-link :to="{ path: 'unitReport', query: { property_id: item.unit_count }}">{{ item.unit_count }}</nuxt-link></td>
          <td>{{ item.budgetted_rent }}</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</template>
<script>
  export default {
    data () {
      return {
          propertyReport: [],
      }
    },
      created () {
      this.initialize();
    },
    methods: {
         async  initialize () {
        try {
            const res = await this.$axios.get('realestate/report/property');
            this.propertyReport=res.data.data
          } catch (e) {
            console.error(e);
          }
      },
    },
  }
</script>