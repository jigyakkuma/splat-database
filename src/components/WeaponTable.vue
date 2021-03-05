<template>
  <v-card>
    <v-card-title>
      ブキ一覧
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="tableHeaders"
      :items="weapons"
      :search="search"
      disable-pagination
      hide-default-footer
      @click:row="selectRow"
    >
    </v-data-table>
  </v-card>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  components: {},
  props: {
    weapons: {
      type: Array,
    },
    selectRow: {
      type: Function,
    },
  },

  data() {
    return {
      search: '',
      tableHeaders: [
        { text: 'ブキ', value: 'weapon_name', sortable: false },
        { text: 'メインカテゴリ', value: 'main_category' },
        { text: 'サブ', value: 'sub_weapon' },
        { text: 'スペシャル', value: 'special_weapon' },
        { text: '必要SP', value: 'special_point' },
      ],
    };
  },
});
</script>
