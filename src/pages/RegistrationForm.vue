<template>
  <q-page class="flex flex-center" padding style="background: linear-gradient(120DEG, #353b46, #35495E)">
    <div class="row justify-center">
      <q-stepper class="bg-grey-1" v-model="step" ref="stepper" color="primary" animated>
        <q-step
          :name="1"
          title="Dados Pessoais"
          done-icon="done"
          active-icon="person_pin"
          :done="step > 1"
          style="min-height: 200px"
        >
          <div class="row justify-center">
            <div
              class="col-xl-8 col-lg-8 col-md-10 col-sm-12 col-xs-12 q-gutter-y-md q-mt-md"
            >
              <q-input label="Name*" outlined v-model="form.name" />
              <q-input label="Sobrenome*" outlined v-model="form.lastName" />
              <p class="text-caption text-bold text-primary">
                * Campos obrigatórios
              </p>
            </div>
          </div>
        </q-step>

        <q-step
          :name="2"
          title="País"
          icon="location_off"
          active-icon="edit_location"
          done-icon="where_to_vote"
          :done="step > 2"
          style="min-height: 200px"
        >
          <div class="row justify-center">
            <div
              class="col-xl-8 col-lg-8 col-md-10 col-sm-12 col-xs-12 q-gutter-y-md q-mt-md"
            >
              <q-select
                label="País*"
                outlined
                option-label="abreviado"
                :options="allCountry"
                v-model="form.selectCountry"
              />
              <p class="text-caption text-bold text-primary">* Campo obrigatório</p>
            </div>
          </div>
        </q-step>

        <q-step
          :name="3"
          title="Endereço"
          icon="home"
          active-icon="home"
          done-icon="house"
          :done="step > 3"
        >
          <div class="row justify-center">
            <div
              class="col-xl-8 col-lg-8 col-md-10 col-sm-12 col-xs-12 q-gutter-y-md q-mt-md"
            >
              <q-input
                @blur="searchCep(form.cep)"
                label="CEP*"
                outlined
                v-model="form.cep"
                mask="#####-###"
                unmasked-value
              />
              <q-input label="Cidade*" outlined v-model="form.city" />
              <q-input label="Endereço*" outlined v-model="form.address" />
              <q-input label="Número*" outlined v-model="form.number" />
              <p class="text-caption text-bold text-primary">
                * Campos obrigatórios
              </p>
            </div>
          </div>
        </q-step>

        <q-step
          :name="4"
          title="Preferência musical"
          icon="music_off"
          music_note
          active-icon="audiotrack"
          done-icon="queue_music"
          :done="step > 4"
        >
          <div class="row justify-center">
            <div
              class="col-xl-8 col-lg-8 col-md-10 col-sm-12 col-xs-12 q-gutter-y-md q-mt-md"
            >
              <p class="text-grey-8 text-h6">
                Selecione o seu estilo musical preferido.
              </p>
              <q-option-group
                :options="optionsMusic"
                type="checkbox"
                v-model="form.selectMusicStyle"
              />
            </div>
          </div>
        </q-step>

        <q-step
          :name="5"
          title="Sistema Operacional"
          icon="desktop_windows"
          active-icon="add_to_queue"
          done-icon="dvr"
          :done="step > 5"
        >
          <div class="row justify-center">
            <div
              class="row col-xl-8 col-lg-8 col-md-10 col-sm-12 col-xs-12 q-gutter-y-md q-mt-md"
            >
              <p class="col-12 text-h6 text-grey-8">Qual seu sistema operacional?</p>
              <q-radio
                class="col-12"
                name="shape"
                v-model="form.selectOS"
                val="windows"
                label="Windows"
              />
              <q-radio
                class="col-12"
                name="shape"
                v-model="form.selectOS"
                val="linux"
                label="Linux"
              />
              <q-radio
                class="col-12"
                name="shape"
                v-model="form.selectOS"
                val="macOs"
                label="MacOS"
              />
            </div>
          </div>
        </q-step>

        <q-step
          :name="6"
          title="Finalizado"
          icon="check"
          active-icon="check"
          done-icon="check"
          :done="step > 6"
        >
          <div class="row justify-center">
            <div class="row col-xl-8 col-lg-8 col-md-10 col-sm-12 col-xs-12 q-gutter-y-md q-mt-md">
              <p class="col-12 text-h6 text-grey-8 text-center">Obrigado pela sua colaboração</p>
            </div>
          </div>
        </q-step>

        <template v-slot:navigation>
          <q-stepper-navigation class="row justify-center">
            <q-btn
              v-if="step > 1 && step <= 5"
              no-caps
              flat
              color="info"
              @click="$refs.stepper.previous()"
              label="Voltar"
              class="q-ml-sm"
            />
            <q-btn
              v-if="step <= 5"
              no-caps
              @click="allChecks()"
              color="primary"
              :label="step === 5 ? 'Finalizar' : 'Continuar'"
            />
          </q-stepper-navigation>
        </template>
      </q-stepper>
    </div>

    <modal-rating ref="modalRating" />
  </q-page>
</template>

<script>

import { ref } from "vue"
import modalRating from 'src/components/modalRating.vue'
import axios from 'axios'

export default {
  components: { modalRating },

  name: "PageIndex",
  setup() {
    return {
      step: ref(1),
      form: ref({
        name: "",
        lastName: "",
        selectCountry: "",
        selectMusicStyle: [],
        cep: "",
        city: "",
        address: "",
        number: "",
        selectOS: "",
      }),
      optionsMusic: [
        { label: "Rock", value: "rock" },
        { label: "Reggae", value: "reggae" },
        { label: "Sertanejo", value: "sertanejo" },
        { label: "Funk", value: "funk" },
        { label: "Axé", value: "axe" },
        { label: "Samba", value: "samba" },
      ],
      allCountry: ref([]),
    };
  },

  async mounted() {
    await axios.get("https://servicodados.ibge.gov.br/api/v1/paises/").then(async (res) => {
      await res.data.forEach(element => {
        this.allCountry.push({...element.nome})
      })
    })
  },

  methods: {
    searchCep(cep) {
      axios.get("https://viacep.com.br/ws/"+cep+"/json/").then(async (res) => {
        this.form.city = res.data.localidade
      })
    },
    allChecks() {
      if (this.step === 1) {
        this.checkStepersOne();
      } else if (this.step === 2) {
        this.checkStepersTwo();
      } else if (this.step === 3) {
        this.checkStepersThree();
      } else if (this.step === 4) {
        this.checkStepersFour();
      } else if (this.step === 5) {
        this.checkStepersFive();
      }
    },
    checkStepersOne() {
      let message = null;
      if (this.step === 1 && this.form.name === "" || this.form.lastName === "") {
        message = "Verifique as informações novamente.";
      }
      if (message === null) {
        this.$refs.stepper.next();
      } else {
        this.messageNotifyError(message);
      }
    },

    checkStepersTwo() {
      let message = null;
      if (this.step === 2 && this.form.selectCountry === "") {
        message = "Informe o seu País.";
      }
      if (message === null) {
        this.$refs.stepper.next();
      } else {
        this.messageNotifyError(message);
      }
    },

    checkStepersThree() {
      let message = null;
      if (this.step === 3 && this.form.cep === "" || this.form.cep.length !== 8 || this.form.city === "" || this.form.address === "" || this.form.number === "") {
        message = "Verifique se todos os campos estão preenchidos corretamente.";
      }
      if (message === null) {
        this.$refs.stepper.next();
      } else {
        this.messageNotifyError(message);
      }
    },

    checkStepersFour() {
      let message = null;
      if (this.step === 4 && this.form.selectMusicStyle.length === 0) {
        message = "Informe qual seu estilo musical preferido.";
      }
      if (message === null) {
        this.$refs.stepper.next();
      } else {
        this.messageNotifyError(message);
      }
    },

    checkStepersFive() {
      let message = null;
      if (this.step === 5 && this.form.selectOS.length === 0) {
        message = "Informe qual o seu sistema operacional.";
      }
      if (message === null) {
        console.log(this.form)
        this.openModalRating()
        this.$refs.stepper.next();
      } else {
        this.messageNotifyError(message);
      }
    },

    messageNotifyError(message) {
      this.$q.notify({
        position: "bottom",
        color: "negative",
        textColor: "white",
        icon: "error",
        message: message,
      });
    },

    openModalRating() {
      this.$refs.modalRating.open()
    }
  },
};
</script>
