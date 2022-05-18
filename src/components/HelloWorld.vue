<template>
  <div class="col-md-6 mx-auto">
    <div class="mb-5">
      <label class="form-label">Selecione o Estado</label>
      <select class="form-select" @change="buscaMunicipios" v-model="estadoSelected">
        <option value="">Estados</option>
        <option v-for="estado in estados" :key="estado.id" :value="estado.sigla"> {{ estado.nome }}</option>
      </select>
      <label class="">Selecione a cidade</label>
      <select class="form-select" @change="buscaMunicipios" v-model="municipioSelected">
        <option value="">Municipios</option>
        <option v-for="municipio in municipios" :key="municipio.id" :value="municipio.nome"> {{ municipio.nome }}</option>
      </select>
      <label class="">Referência</label>
      <input class="form-control" v-model="referencia" placeholder="Digite a sua referência" />
      <button class="btn btn-primary mt-4" @click="buscarCeps">Buscar</button>
      <div v-if="ceps.length > 0">
        <h1>Ceps Encontrados Pelo Buscador</h1>
        <ul>
          <li v-for="cep in ceps" :key="cep.cep" @click="mostrarModal()">
            CEP: {{ cep.cep }} - Bairro: {{ cep.bairro }} - Logradouro:
            {{ cep.logradouro }}
            <Popup v-if="popupTriggers.buttonTrigger" :TogglePopup="() => TogglePopup('buttonTrigger')" :cep="cep.cep" >
            </Popup>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { ref } from 'vue';
import Popup from './Popup.vue';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      estados: [],
      municipios: [],
      estadoSelected: '',
      municipioSelected: '',
      referencia: '',
      ceps: []
    }
  },
  mounted() {
    this.buscaEstados();
  },
  setup() {
    const popupTriggers = ref({
      buttonTrigger: false,
      timedTrigger: false
    });

    const TogglePopup = (trigger) => {
      popupTriggers.value[trigger] = !popupTriggers.value[trigger]

    }

    setTimeout(() => {
      popupTriggers.value.timedTrigger = true;
    }, 3000);

    return {
      Popup,
      popupTriggers,
      TogglePopup
    }
  },

  methods: {
    buscaEstados() {
      axios.get('https://servicodados.ibge.gov.br/api/v1/localidades/estados')
        .then(response => {
          this.estados = response.data;
        })
        .catch(error => {
          console.log(error);
        })
    },
    buscaMunicipios() {
      axios.get('https://servicodados.ibge.gov.br/api/v1/localidades/estados/' + this.estadoSelected + '/municipios')
        .then(response => {
          this.municipios = response.data;
        })
        .catch(error => {
          console.log(error);
        })
    },
    buscarCeps() {
      axios.get(`https://viacep.com.br/ws/${this.estadoSelected}/${this.municipioSelected}/${this.referencia}/json/`)
        .then(response => { this.ceps = response.data })
    },
    mostrarModal() {
      this.TogglePopup('buttonTrigger')
    }
  },
  components: {
    Popup
  }
}
</script>
