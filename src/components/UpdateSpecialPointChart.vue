<template>
  <div>
    <chart :chartData="chartData" :options="options"></chart>
    <h4>スペシャルポイント増加量</h4>
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue';
import Chart from './Chart.vue';
import { ChartData, ChartOptions } from 'chart.js';
import { TypeWeapon } from '@/types/TypeWeapon.ts';
import { TypeUpdateSpecialPoint } from '@/types/TypeUpdateSpecialPoint.ts';
import { TypeVersion } from '@/types/TypeVersion.ts';

export default Vue.extend({
  components: { Chart },

  props: {
    weapons: {
      type: Array as PropType<TypeWeapon[]>,
    },
    updateSpecialPoints: {
      type: Array as PropType<TypeUpdateSpecialPoint[]>,
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

      const datasSpecialWeapon: number[] = this.pickWeaponData(
        this.updateSpecialPoints,
        weapon.weapon_name,
        versions,
        weapon.special_point
      );

      return {
        labels: versions,
        datasets: [
          {
            label: weapon.weapon_name,
            data: this.dataSum(datasSpecialWeapon),
            borderColor: 'rgba(60, 190, 20, 0.8)',
            backgroundColor: 'rgba(60, 190, 20, 0.3)',
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
              ticks: {
                beginAtZero: true,
                min: 0,
                max: 250,
              },
            },
          ],
        },
      };
    },
  },
  methods: {
    pickWeaponData(
      data: Array<TypeUpdateSpecialPoint>,
      weaponName: string,
      versions: string[],
      currentSpecialPoint: number
    ): number[] {
      const datasWeapon: number[] = Array(versions.length);
      datasWeapon.fill(0);

      const filterData = data.filter(function(item) {
        if (item.weapon_name == weaponName) return true;
      });
      console.log(filterData);
      if (filterData.length == 0) {
        datasWeapon[0] = currentSpecialPoint;
        return datasWeapon;
      }

      // datasWeaponの先頭はv1.0.0で初期値となるため強引な処理は許容
      datasWeapon[0] = filterData[0].before_special_point;
      for (const item of filterData) {
        const index = versions.indexOf(item.version);
        datasWeapon[index] = item.after_special_point;
      }

      return datasWeapon;
    },
    dataSum(datas: number[]): number[] {
      let point = datas[0]; // datas[0]には必ず初期値がセットされているので強引にこうしている
      const datasSum: number[] = Array(datas.length);
      for (let i = 0; i < datasSum.length; i++) {
        if (datas[i] > 0) point = datas[i];
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
