<template>
  <select
    @change="onChange"
    class="form-select mt-10 block w-full border p-3 rounded-none"
    v-model="selected"
  >
    <option value="0">Select Country</option>
    <option v-for="country in countries" :value="country.ID" :key="country.ID">
      {{ country.Country }}
    </option>
  </select>
</template>

<script>
export default {
  name: 'CountrySelect',
  props: ['countries'],
  data: function() {
    return {
      selected: 0,
    };
  },

  methods: {
    onChange: function() {
      const country = this.countries.find(item => item.ID === this.selected);

      this.$emit('get-country', country);
    },
  },
};
</script>
