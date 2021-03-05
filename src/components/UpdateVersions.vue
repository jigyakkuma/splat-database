<template>
  <div>
    <h5>{{ hoge }}</h5>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'UpdateVersions',
  props: {
    weapons: {
      type: Array,
    },
    selectWeaponId: {
      type: Number,
    },
  },
  computed: {
    hoge(): string {
      const weapon = this.weapons[this.selectWeaponId];
      return 'hoge';
    },
  },
});
</script>
