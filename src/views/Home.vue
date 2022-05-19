<template>
  <div class="container">
    <table>
      <thead>
        <tr>
          <th scope="col">Aliment</th>
          <th scope="col">Masse</th>
          <th scope="col">Impact Aliment</th>
          <th scope="col">Transport</th>
          <th scope="col">Distance</th>
          <th scope="col">Impact Transport</th>
          <th scope="col">Cuisson</th>
          <th scope="col">Durée</th>
          <th scope="col">Impact Cuisson</th>
          <th scope="col">Impact environnemental</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="index in indexes" :key="index">
          <td>
            <select name="aliment-list" v-model="alimentSelected[index]">
              <option
                :value="aliment.name"
                v-for="aliment in aliments"
                :key="aliment.name"
              >
                {{ aliment.name }}
              </option>
            </select>
          </td>
          <td>
            <input
              type="text"
              placeholder=""
              v-model="alimentQuantity[index]"
            />
          </td>
          <td>{{ impactAliment(index) }}</td>
          <td>
            <select name="transport-list" v-model="transportSelected[index]">
              <option
                :value="transport.name"
                v-for="transport in transports"
                :key="transport.name"
              >
                {{ transport.name }}
              </option>
            </select>
          </td>
          <td>
            <input type="text" v-model="transportDistance[index]" />
          </td>
          <td>{{ impactTransport(index) }}</td>
          <td>
            <select name="cuisson-list" v-model="cuissonSelected[index]">
              <option
                selected
                :value="cuisson.name"
                v-for="cuisson in cuissons"
                :key="cuisson.name"
              >
                {{ cuisson.name }}
              </option>
            </select>
          </td>
          <td>
            <input type="text" v-model="cuissonDuree[index]" />
          </td>
          <td>{{ impactCuisson(index) }}</td>
          <td>{{ impactTotal(index) }}</td>
        </tr>
        <tr>
          <td colspan="9">
            <i class="fas fa-plus-circle" @click="addLine()"></i>
          </td>
        </tr>
      </tbody>

      <tfoot>
        <tr>
          <td></td>
          <td>{{ sommes.masses }}</td>
          <td>{{ sommes.impactAliments }}</td>
          <td></td>
          <td>{{ sommes.distances }}</td>
          <td>{{ sommes.impactTransports }}</td>
          <td></td>
          <td>{{ sommes.cuissons }}</td>
          <td>{{ sommes.impactCuissons }}</td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      aliments: [
        {
          name: 'Boeuf',
          impact: 10,
        },
        {
          name: 'Poulet',
          impact: 6,
        },
        {
          name: 'Pommes de terre',
          impact: 4,
        },
        {
          name: 'Courgettes',
          impact: 2,
        },
        {
          name: 'Riz',
          impact: 2,
        },
      ],
      transports: [
        { name: 'Camion', impact: 1 },
        { name: 'Bateau', impact: 0.001 },
        { name: 'Avion', impact: 5 },
      ],
      cuissons: [
        { name: 'Plaque gaz', impact: 0.1 },
        { name: 'Four électrique', impact: 0.2 },
      ],
      alimentQuantity: [],
      transportDistance: [],
      cuissonDuree: [],
      alimentSelected: [],
      transportSelected: [],
      cuissonSelected: [],
      list: [],
      indexes: 1,
    };
  },
  methods: {
    impactAliment(index) {
      if (!this.alimentSelected[index]) return 0;
      console.log(this.alimentSelected[index]);
      const aliment = this.aliments.find(
        (item) => item.name === this.alimentSelected[index],
      );
      const resultatAliment = this.alimentQuantity[index] * aliment.impact;
      return resultatAliment;
    },
    impactTransport(index) {
      if (!this.transportSelected[index]) return 0;
      const transport = this.transports.find(
        (item) => item.name === this.transportSelected[index],
      );
      const resultatTransport =
        this.transportDistance[index] * transport.impact;
      return resultatTransport;
    },
    impactCuisson(index) {
      if (!this.cuissonSelected[index]) return 0;
      const cuisson = this.cuissons.find(
        (item) => item.name === this.cuissonSelected[index],
      );
      const resultatCuisson = this.cuissonDuree[index] * cuisson.impact;
      return resultatCuisson;
    },
    impactTotal(index) {
      if (
        !this.alimentSelected[index] ||
        !this.transportSelected[index] ||
        !this.cuissonSelected[index]
      ) {
        return 0;
      }
      const resultatTotal =
        this.impactAliment(index) +
        this.impactTransport(index) +
        this.impactCuisson(index);
      return resultatTotal.toFixed(2);
    },
    addLine() {
      this.indexes += 1;
    },
  },
  computed: {
    sommes() {
      return {
        masses: this.alimentQuantity.reduce((a, b) => +a + +b, 0),
        impactAliments:
          this.impactAliment(0) +
          this.impactAliment(1) +
          this.impactAliment(2) +
          this.impactAliment(3) +
          this.impactAliment(4),
        distances: this.transportDistance.reduce((a, b) => +a + +b, 0),
        impactTransports:
          this.impactTransport(0) +
          this.impactTransport(1) +
          this.impactTransport(2) +
          this.impactTransport(3) +
          this.impactTransport(4),
        cuissons: this.cuissonDuree.reduce((a, b) => +a + +b, 0),
        impactCuissons:
          this.impactCuisson(0) +
          this.impactCuisson(1) +
          this.impactCuisson(2) +
          this.impactCuisson(3) +
          this.impactCuisson(4),
      };
    },
  },
};
</script>

<style scoped>
.container {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}

.container i {
  padding-left: 8px;
}
</style>
