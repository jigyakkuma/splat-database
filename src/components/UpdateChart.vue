<template>
  <div>
    <chart :chartData="chartData" :options="options"></chart>
    <h4>アップデートの変移</h4>
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue';
import Chart from './Chart.vue';
import { ChartData, ChartOptions } from 'chart.js';
import { TypeWeapon } from '@/types/TypeWeapon.ts';
import { TypeUpdateWeapon } from '@/types/TypeUpdateWeapon.ts';
import { TypeVersion } from '@/types/TypeVersion.ts';

export default Vue.extend({
  components: { Chart },

  props: {
    weapons: {
      type: Array as PropType<TypeWeapon[]>,
    },
    updateMainWeapons: {
      type: Array as PropType<TypeUpdateWeapon[]>,
    },
    updateSubWeapons: {
      type: Array as PropType<TypeUpdateWeapon[]>,
    },
    updateSpecialWeapons: {
      type: Array as PropType<TypeUpdateWeapon[]>,
    },
    versions: {
      type: Array as PropType<TypeVersion[]>,
    },
    selectWeaponId: {
      type: Number,
    },
  },

  computed: {
    chartData(): ChartData {
      const versions: string[] = [];
      for (const item of this.versions) {
        versions.push(item.version);
      }

      const weapon: TypeWeapon = this.selectWeapon();

      const datasMainWeapon: number[] = this.pickWeaponData(
        this.updateMainWeapons,
        weapon.main_weapon,
        versions
      );
      const datasSubWeapon: number[] = this.pickWeaponData(
        this.updateSubWeapons,
        weapon.sub_weapon,
        versions
      );
      const datasSpecialWeapon: number[] = this.pickWeaponData(
        this.updateSpecialWeapons,
        weapon.special_weapon,
        versions
      );

      return {
        labels: versions,
        datasets: [
          {
            label: weapon.main_weapon,
            data: this.dataSum(datasMainWeapon),
            borderColor: 'rgba(60, 190, 220, 0.8)',
            backgroundColor: 'rgba(60, 190, 220, 0.3)',
            lineTension: 0.5,
            pointRadius: 0,
          },
          {
            label: weapon.sub_weapon,
            data: this.dataSum(datasSubWeapon),
            borderColor: 'rgba(220, 60, 190, 0.8)',
            backgroundColor: 'rgba(220, 60, 190, 0.3)',
            lineTension: 0.5,
            pointRadius: 0,
          },
          {
            label: weapon.special_weapon,
            data: this.dataSum(datasSpecialWeapon),
            borderColor: 'rgba(190, 220, 60, 0.8)',
            backgroundColor: 'rgba(190, 220, 60, 0.3)',
            lineTension: 0.5,
            pointRadius: 0,
          },
        ],
      };
    },
    options(): ChartOptions {
      return {
        scales: {
          yAxes: [
            {
              stacked: true,
            },
          ],
        },
        tooltips: {
          callbacks: {
            title: () => {
              return new Date().toLocaleTimeString();
            },
          },
        },
      };
    },
  },
  methods: {
    pickWeaponData(
      data: Array<TypeUpdateWeapon>,
      weaponName: string,
      versions: string[]
    ): number[] {
      const datasWeapon: number[] = Array(versions.length);
      datasWeapon.fill(0);

      const filterData = data.filter((item) => {
        if (item.weapon_name == weaponName) return true;
      });
      if (filterData.length == 0) {
        return datasWeapon;
      }

      for (const item of filterData) {
        const p = item.updown == 'up' ? 1 : -1;
        const index = versions.indexOf(item.version);
        datasWeapon[index] += p;
      }
      return datasWeapon;
    },
    dataSum(datas: number[]): number[] {
      let point = 10;
      const datasSum: number[] = Array(datas.length);
      for (let i = 0; i < datasSum.length; i++) {
        point += datas[i];
        datasSum[i] = point;
      }
      return datasSum;
    },
    selectWeapon(): TypeWeapon {
      let weapon!: TypeWeapon;
      for (const item of this.weapons) {
        if (item.weapon_id == this.selectWeaponId) {
          weapon = item;
          break;
        }
      }
      return weapon;
    },
  },
});
</script>
