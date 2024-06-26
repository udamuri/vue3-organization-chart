<template>
  <table class="table">
    <tbody>
      <tr>
        <td
          :colspan="
            datasource.children && datasource.children.length
              ? datasource.children.length * 2
              : null
          "
        >
          <div
            class="chartNode"
            :class="datasource.gender == 'm' ? 'cnMale' : 'cnFemale'"
            style="min-width: 16rem"
            :id="datasource.id"
            @click.stop="handleClick(datasource)"
          >
            <slot :node-data="datasource">
              <div :class="datasource.gender == 'm' ? 'chartTitleMale' : 'chartTitleFemale'">
                <p style="margin-left: 0.5rem; margin-right: 0.5rem">
                  {{ datasource.name }}
                </p>
              </div>
              <div :class="datasource.gender == 'm' ? 'chartContentMale' : 'chartContentFemale'">
                <div style="margin-left: 0.5rem; margin-right: 0.5rem">
                  <img :src="datasource?.image" class="image_profile" />
                </div>
                <div class="grid grid-cols-3 gap-1 font-sm" :class="datasource.gender == 'm' ? 'chartFooterMale' : 'chartFooterFemale'">
                  <div style="border-right: 2px solid #fff;" class="col-span-2">
                    <template v-if="datasource?.birth_date && datasource?.date_of_death">
                      {{ datasource?.birth_date }}/{{ datasource?.date_of_death }}
                    </template>
                    <template v-if="datasource?.birth_date && !datasource?.date_of_death">
                      Lahir {{ datasource?.birth_date }}
                    </template>
                    <template v-if="!datasource?.birth_date && datasource?.date_of_death">
                      Meninggal {{ datasource?.date_of_death }}
                    </template>
                    </div>
                  <div>{{ datasource?.age }}</div>
                </div>
              </div>
            </slot>
          </div>
        </td>
      </tr>
      <template v-if="datasource.children && datasource.children.length">
        <tr class="chartLines">
          <td :colspan="datasource.children.length * 2">
            <div class="chartDownLine"></div>
          </td>
        </tr>
        <tr class="chartLines">
          <td class="chartRightLine"></td>
          <template v-for="n in datasource.children.length - 1" v-bind:key="n">
            <td class="chartLeftLine chartTopLine"></td>
            <td class="chartRightLine chartTopLine"></td>
          </template>
          <td class="chartLeftLine"></td>
        </tr>
        <tr class="nodes">
          <td colspan="2" v-for="child in datasource.children" :key="child.id">
            <node :datasource="child" :handle-click="handleClick">
              <template
                v-for="slot in Object.keys($slots)"
                v-slot:[slot]="scope"
              >
                <slot :name="slot" v-bind="scope" />
              </template>
            </node>
          </td>
        </tr>
      </template>
    </tbody>
  </table>
</template>

<script>
/* eslint-disable */
export default {
  name: "node",
  props: {
    datasource: Object,
    handleClick: Function,
  },
  methods: {},
};
</script>

<style>
.table {
  margin-bottom: 0.5rem;
  margin-left: auto;
  margin-right: auto;
  border-collapse: separate;
}

.chartNode {
  box-sizing: border-box;
  display: inline-flex;
  flex-direction: column;
  position: relative;
  margin: 0 1px 2px 1px;
  max-width: 20px;
  border: 1px solid #054232;
  text-align: center;
  transition-property: color, background-color, border-color,
    text-decoration-color, fill, stroke, opacity, box-shadow, transform, filter,
    backdrop-filter;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 300ms;
  border-radius: 0.35rem;
}

.cnMale {
  border: 1px solid #ff0000 !important;
}

.cnFemale {
  border: 1px solid #054232  !important;
}

.cnMale:hover {
  box-shadow: 0 0 5px #ff0000;
  cursor: pointer;
  z-index: 20;
}

.cnFemale:hover {
  box-shadow: 0 0 5px #054232;
  cursor: pointer;
  z-index: 20;
}

.chartTitleMale {
  text-align: center;
  font-size: 0.75rem;
  font-weight: bold;
  line-height: 1.25rem;
  overflow: hidden;
  white-space: nowrap;
  background: #ff0000;
  color: white;
  border-top-left-radius: 0.25rem;
  border-top-right-radius: 0.25rem;
}

.chartTitleFemale {
  text-align: center;
  font-size: 0.75rem;
  font-weight: bold;
  line-height: 1.25rem;
  overflow: hidden;
  white-space: nowrap;
  background: #054232 !important;
  color: white;
  border-top-left-radius: 0.25rem;
  border-top-right-radius: 0.25rem;
}

.chartContentMale {
  box-sizing: border-box;
  width: 100%;
  height: fit-content !important;
  font-size: 0.75rem;
  line-height: 1rem;
  border: #ff0000;
  text-align: center;
  border-bottom-right-radius: 0.25rem;
  border-bottom-left-radius: 0.25rem;
  background: white;
  color: black;
  overflow: hidden;
  white-space: nowrap;
}

.chartContentFemale {
  box-sizing: border-box;
  width: 100%;
  height: fit-content !important;
  font-size: 0.75rem;
  line-height: 1rem;
  border: #054232 !important;
  text-align: center;
  border-bottom-right-radius: 0.25rem;
  border-bottom-left-radius: 0.25rem;
  background: white;
  color: black;
  overflow: hidden;
  white-space: nowrap;
}

.chartFooterMale {
  text-align: center;
  font-size: 0.75rem;
  font-weight: bold;
  line-height: 1.25rem;
  overflow: hidden;
  white-space: nowrap;
  background: #d8d8d8 ;
  color: #ff0000;
  border-bottom-left-radius: 0.25rem;
  border-bottom-right-radius: 0.25rem;
}

.chartFooterFemale {
  text-align: center;
  font-size: 0.75rem;
  font-weight: bold;
  line-height: 1.25rem;
  overflow: hidden;
  white-space: nowrap;
  background: #d8d8d8 !important;
  color: #054232;
  border-bottom-left-radius: 0.25rem;
  border-bottom-right-radius: 0.25rem;
}

.chartLines {
  height: 1.25rem;
}

.chartDownLine {
  background: #054232;
  margin-left: auto;
  margin-right: auto;
  height: 1.25rem;
  width: 0.125rem;
  float: none;
}

.chartTopLine {
  border-top-color: #054232;
  border-top-style: solid;
  border-top-width: 2px;
}

.chartRightLine {
  border-right-color: #054232;
  border-right-style: solid;
  border-right-width: 1px;
}

.chartLeftLine {
  border-left-color: #054232;
  border-left-style: solid;
  border-left-width: 1px;
}

.image_profile {
  display: block;
  margin-left: auto;
  margin-right: auto;
  object-fit: cover;
  width: 110px !important;
  height: 150px !important;
}
</style>
