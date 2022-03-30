<template>
<Layout>
   
  <div class= "form-group">
      <label for="">
          <h3>Selecciona un indicador</h3>
      </label>
      <select v-model="indicador" class="form-select" name="indicador">
          <option v-for="(indicador, index) of indicadoresEconomicos" :key="index">
              {{indicador.codigo}}
          </option>
      </select>
      <Spinner v-if="loading"/>
        <!-- Indicadores Informacion -->
      <div class= "mt-4" v-if="indicadorInfo.serie">
         

          <div class= "mt-4">
           <h4> Nombre del Indicador : {{indicadorInfo.nombre | transformarMayus}} </h4>
           <h5>Unidad de medida : {{indicadorInfo.unidad_medida}} </h5>   

          </div>
      </div>
      <!-- tabla con la informacion del indicador seleccionado -->
      <table v-if="indicadorInfo.serie" class= "table mt-5">
          <thead>
              <tr>
                  <th>Fecha</th>
                  <th>Valor</th>
              </tr>
          </thead>

          <tbody>
              <tr v-for="indice of indicadorInfo.serie" :key="indice.fecha" >
                <td>{{indice.fecha | fechaFilter}} </td>
                <td> {{indice.valor}} </td>
              </tr>


          </tbody>

      </table>

      </div> 
 </Layout>

</template>

<script>
import Layout from '../layout/Layout.vue';
import Spinner from '../components/Spinner.vue';
import {indicadoresEconomicos} from '../api/indicadores';

export default {
    name: 'Homeview',
    components:{
        Layout,
        Spinner
    },
    data(){
        return {
            indicadorInfo: [],
            indicadoresEconomicos: [
                { codigo: 'uf'},
                { codigo: 'ivp'},
                { codigo: 'dolar'},
                { codigo: 'dolar_intercambio'},
                { codigo: 'euro'},
                { codigo: 'ipc'},
                { codigo: 'imacec'},
                { codigo: 'tpm'},
                { codigo: 'libra_cobre'},
                { codigo: 'tasa_desempleo'},
                { codigo: 'bitcoin'},

            ],
            indicador: '',
            loading: false,
            mifecha: ''
        }
    },
    methods: {
        async getIndicadores() {
            try {
                this.indicadorInfo = await indicadoresEconomicos(this.indicador);

                console.log(this.indicadorInfo.serie); //array
                this.loading = false;
                
            } catch (error) {
                console.log(error);
            }

        },


    },
    created() {
        this.getIndicadores();
    },
    watch: {
        indicador () {
            this.loading = true;
            this.indicadorInfo = [];          
            this.getIndicadores();
        }
    },
    filters : {
        fechaFilter: function(value) {
            let fecha =  new Date(value);
            //value= indice.fecha
            let formatoFecha = `${fecha.getDate()} / ${fecha.getMonth()+1}/${fecha.getFullYear()}`
            return formatoFecha;
        },

        transformarMayus: function(str) {
            //str = indicadorInfo.nombre
            return str.toUpperCase();
        }
     },
    



};
</script>

<style>
</style>