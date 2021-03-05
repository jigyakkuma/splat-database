<template>
  <v-app>
    <v-main>
      <v-row>
        <v-col>
          <v-row class="mt-6" justify="center">
            <update-chart
              :weapons="weapons"
              :updateMainWeapons="updateMainWeapons"
              :updateSubWeapons="updateSubWeapons"
              :updateSpecialWeapons="updateSpecialWeapons"
              :versions="versions"
              :selectWeaponId="weaponId"
            ></update-chart>
          </v-row>
          <v-row class="mt-6" justify="center">
            <update-special-point-chart
              :weapons="weapons"
              :updateSpecialPoints="updateSpecialPoints"
              :versions="versions"
              :selectWeaponId="weaponId"
            >
            </update-special-point-chart>
          </v-row>
        </v-col>
        <v-col>
          <v-row class="mt-6" justify="center">
            <weapon-table
              :weapons="weapons"
              :selectRow="selectWeapon"
            ></weapon-table>
          </v-row>
        </v-col>
      </v-row>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import UpdateChart from '@/components/UpdateChart.vue';
import UpdateSpecialPointChart from '@/components/UpdateSpecialPointChart.vue';
import WeaponTable from '@/components/WeaponTable.vue';
import Weapons from '@/assets/JSON/weapon.json';
import UpdateMainWeapons from '@/assets/JSON/update_main.json';
import UpdateSubWeapons from '@/assets/JSON/update_sub.json';
import UpdateSpecialWeapons from '@/assets/JSON/update_special.json';
import UpdateSpecialPoints from '@/assets/JSON/update_special_point.json';
import Versions from '@/assets/JSON/version.json';
import { TypeWeapon } from '@/types/TypeWeapon.ts';
import { TypeUpdateWeapon } from '@/types/TypeUpdateWeapon.ts';
import { TypeUpdateSpecialPoint } from '@/types/TypeUpdateSpecialPoint.ts';
import { TypeVersion } from '@/types/TypeVersion.ts';

export default Vue.extend({
  name: 'App',

  components: {
    UpdateChart,
    UpdateSpecialPointChart,
    WeaponTable,
  },

  data(): {
    weapons: Array<TypeWeapon>;
    updateMainWeapons: Array<TypeUpdateWeapon>;
    updateSubWeapons: Array<TypeUpdateWeapon>;
    updateSpecialWeapons: Array<TypeUpdateWeapon>;
    updateSpecialPoints: Array<TypeUpdateSpecialPoint>;
    versions: Array<TypeVersion>;
    weaponId: number;
  } {
    return {
      weapons: Weapons,
      updateMainWeapons: UpdateMainWeapons,
      updateSubWeapons: UpdateSubWeapons,
      updateSpecialWeapons: UpdateSpecialWeapons,
      updateSpecialPoints: UpdateSpecialPoints,
      versions: Versions,
      weaponId: Weapons[0].weapon_id,
    };
  },

  methods: {
    selectWeapon(row: any) {
      this.weaponId = row.weapon_id;
    },
  },
});
</script>
