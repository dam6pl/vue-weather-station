<template>
  <div>
    <div class="row" v-if="!loading">
      <div class="col-12 my-2">
        <h1>Stacja: {{ this.currentStation.name }} ({{ this.currentStation.latitude }}, {{ this.currentStation.longitude }})</h1>
      </div>
      <div class="col-12">
        <card type="chart">
          <template slot="header">
            <div class="row">
              <div class="col-sm-6 text-left">
                <h5 class="card-category">Dane kolekcjonowane z dwóch czujników: DHT22 oraz MPL115A2</h5>
                <h2 class="card-title">Temperatura</h2>
              </div>
              <div class="col-sm-6">
                <div class="btn-group btn-group-toggle float-right" data-toggle="buttons">
                  <label
                    v-for="(option, index) in chartsCategories"
                    :key="option"
                    class="btn btn-sm btn-primary btn-simple"
                    :class="{active: temperatureChart.activeIndex === index}"
                    :id="index"
                  >
                    <input
                      type="radio"
                      @click="initCharts(index)"
                      name="options"
                      autocomplete="off"
                      :checked="temperatureChart.activeIndex === index"
                    />
                    {{option}}
                  </label>
                </div>
              </div>
            </div>
          </template>
          <div class="chart-area" v-if="!chartLoading">
            <line-chart
              style="height: 100%"
              ref="bigChart"
              chart-id="big-line-chart"
              :chart-data="temperatureChart.chartData"
              :gradient-colors="temperatureChart.gradientColors"
              :gradient-stops="temperatureChart.gradientStops"
              :extra-options="temperatureChart.extraOptions"
            ></line-chart>
          </div>
          <div class="loader" v-if="chartLoading">
            <div class="lds-ellipsis">
              <div></div>
              <div></div>
              <div></div>
              <div></div>
            </div>
          </div>
        </card>
      </div>
    </div>

    <div class="row" v-if="!loading">
      <div class="col-lg-4">
        <card type="chart">
          <template slot="header">
            <h5 class="card-category">Dane kolekcjonowane z czujnika: DHT22</h5>
            <h3 class="card-title">Wilgotność potwietrza</h3>
          </template>
          <div class="chart-area" v-if="!chartLoading">
            <line-chart
              style="height: 100%"
              chart-id="purple-line-chart"
              :chart-data="humidityChart.chartData"
              :gradient-colors="humidityChart.gradientColors"
              :gradient-stops="humidityChart.gradientStops"
              :extra-options="humidityChart.extraOptions"
            ></line-chart>
          </div>
          <div class="loader" v-if="chartLoading">
            <div class="lds-ellipsis">
              <div></div>
              <div></div>
              <div></div>
              <div></div>
            </div>
          </div>
        </card>
      </div>
      <div class="col-lg-4">
        <card type="chart">
          <template slot="header">
            <h5 class="card-category">Dane kolekcjonowane z czujnika: MPL115A2</h5>
            <h3 class="card-title">Ciśnienie atmosferyczne</h3>
          </template>
          <div class="chart-area" v-if="!chartLoading">
            <bar-chart
              style="height: 100%"
              chart-id="purple-line-chart"
              :chart-data="pressureChart.chartData"
              :gradient-colors="pressureChart.gradientColors"
              :gradient-stops="pressureChart.gradientStops"
              :extra-options="pressureChart.extraOptions"
            ></bar-chart>
          </div>
          <div class="loader" v-if="chartLoading">
            <div class="lds-ellipsis">
              <div></div>
              <div></div>
              <div></div>
              <div></div>
            </div>
          </div>
        </card>
      </div>
      <div class="col-lg-4">
        <card type="chart">
          <template slot="header">
            <h5 class="card-category">Dane kolekcjonowane z czujnika: GL5528</h5>
            <h3 class="card-title">Jasność</h3>
          </template>
          <div class="chart-area" v-if="!chartLoading">
            <line-chart
              style="height: 100%"
              chart-id="green-line-chart"
              :chart-data="illuminanceChart.chartData"
              :gradient-colors="illuminanceChart.gradientColors"
              :gradient-stops="illuminanceChart.gradientStops"
              :extra-options="illuminanceChart.extraOptions"
            ></line-chart>
          </div>
          <div class="loader" v-if="chartLoading">
            <div class="lds-ellipsis">
              <div></div>
              <div></div>
              <div></div>
              <div></div>
            </div>
          </div>
        </card>
      </div>
    </div>
  </div>
</template>
<script>
import LineChart from "@/components/Charts/LineChart";
import BarChart from "@/components/Charts/BarChart";
import * as chartConfigs from "@/components/Charts/config";
import config from "@/config";
import axios from "axios";
import { FadeTransition } from "vue2-transitions";

export default {
  components: {
    LineChart,
    BarChart,
    FadeTransition
  },
  data() {
    return {
      currentStation: null,
      loading: true,
      chartLoading: true,
      temperatureChart: {
        activeIndex: 0,
        chartData: {
          datasets: [
            {
              fill: true,
              borderColor: config.colors.primary,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.primary,
              pointBorderColor: "rgba(255,255,255,0)",
              pointHoverBackgroundColor: config.colors.primary,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: []
            }
          ],
          labels: [],
          units: " °C"
        },
        extraOptions: chartConfigs.purpleChartOptions,
        gradientColors: [
          "rgba(76, 211, 150, 0.1)",
          "rgba(53, 183, 125, 0)",
          "rgba(119,52,169,0)"
        ],
        gradientStops: [1, 0.4, 0],
        categories: []
      },
      humidityChart: {
        chartData: {
          datasets: [
            {
              fill: true,
              borderColor: "#e89805",
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: "#e89805",
              pointBorderColor: "rgba(255,255,255,0)",
              pointHoverBackgroundColor: "#e89805",
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: []
            }
          ],
          labels: [],
          units: "%"
        },
        extraOptions: chartConfigs.purpleChartOptions,
        gradientColors: [
          "rgba(232, 152, 5, 0.1)",
          "rgba(232, 152, 5, 0)",
          "rgba(119,52,169,0)"
        ],
        gradientStops: [1, 0.4, 0],
        categories: []
      },
      illuminanceChart: {
        chartData: {
          datasets: [
            {
              fill: true,
              borderColor: "rgb(216, 17, 17)",
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: "rgb(216, 17, 17)",
              pointBorderColor: "rgba(255,255,255,0)",
              pointHoverBackgroundColor: "rgb(216, 17, 17)",
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: []
            }
          ],
          labels: [],
          units: " lm"
        },
        extraOptions: chartConfigs.purpleChartOptions,
        gradientColors: [
          "rgba(216, 17, 17, 0.1)",
          "rgba(216, 17, 17, 0)",
          "rgba(119,52,169,0)"
        ],
        gradientStops: [1, 0.4, 0],
        categories: []
      },
      pressureChart: {
        chartData: {
          datasets: [
            {
              fill: true,
              borderColor: "rgb(3, 169, 244)",
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: "rgb(3, 169, 244)",
              pointBorderColor: "rgba(255,255,255,0)",
              pointHoverBackgroundColor: "rgb(3, 169, 244)",
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: []
            }
          ],
          labels: [],
          units: " hPa"
        },
        extraOptions: chartConfigs.barChartOptions,
        gradientColors: [
          "rgba(3, 169, 244, 0.1)",
          "rgba(3, 169, 244, 0)",
          "rgba(119,52,169,0)"
        ],
        gradientStops: [1, 0.4, 0],
        categories: []
      }
    };
  },
  computed: {
    chartsCategories() {
      return ["Dzień", "Miesiąc", "Rok"];
    }
  },
  methods: {
    initCharts(index) {
      if (this.temperatureChart.activeIndex === index) {
        return;
      }

      this.temperatureChart.activeIndex = index;
      this.chartLoading = true;

      let url = "",
        temperature = [],
        humidity = [],
        illuminance = [],
        pressure = [],
        labels = [];

      switch (index) {
        case 0:
          url =
            process.env.VUE_APP_API_URL +
            "v1/stations/" +
            this.$router.currentRoute.params.id +
            "/measurements?interval=hourly";
          break;
        case 1:
          url =
            process.env.VUE_APP_API_URL +
            "v1/stations/" +
            this.$router.currentRoute.params.id +
            "/measurements?interval=daily";
          break;
        case 2:
          url =
            process.env.VUE_APP_API_URL +
            "v1/stations/" +
            this.$router.currentRoute.params.id +
            "/measurements?interval=monthly";
          break;
      }

      axios.get(url).then(response => {
        if (response.data.success) {
          response.data.data.forEach(el => {
            temperature.push(el.temperature);
            humidity.push(el.humidity);
            illuminance.push(el.illuminance);
            pressure.push(el.pressure);
            switch (index) {
              case 0:
                labels.push(
                  el.created_at
                    .replace(/\d{4}-\d{2}-\d{2}\s/, " ")
                    .replace(/59/g, "00")
                );
                break;
              case 1:
                labels.push(el.created_at.replace(/\s\d{2}:\d{2}:\d{2}/, " "));
                break;
              case 2:
                labels.push(
                  el.created_at.replace(/-\d{2}-\d{2}\s\d{2}:\d{2}:\d{2}/, " ")
                );
                break;
            }
          });

          this.temperatureChart.chartData.datasets[0].data = temperature.reverse();
          this.humidityChart.chartData.datasets[0].data = humidity.reverse();
          this.illuminanceChart.chartData.datasets[0].data = illuminance.reverse();
          this.pressureChart.chartData.datasets[0].data = pressure.reverse();

          this.temperatureChart.chartData.labels = this.humidityChart.chartData.labels = this.illuminanceChart.chartData.labels = this.pressureChart.chartData.labels = labels.reverse();

          this.chartLoading = false;
        }
      });
    }
  },
  mounted() {
    axios
      .get(
        process.env.VUE_APP_API_URL +
          "v1/stations/" +
          this.$router.currentRoute.params.id
      )
      .then(response => {
        if (response.data.success) {
          this.currentStation = response.data.data;
          this.loading = false;
        }
      })
      .catch(error => {
        this.$router.push("/404");
      });

    this.initCharts(1);
  }
};
</script>
<style>
</style>
