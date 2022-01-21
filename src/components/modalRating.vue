<template>
  <q-dialog persistent v-model="modalRating">
    <q-card style="width: 300px">
      <div class="row justify-center q-pa-md q-gutter-y-md">
        <span class="col-12 text-center text-bold text-grey-8"> Quanto você ficou satisfeito com o atendimento? </span>
        <q-form @submit.prevent="userExperience" class="q-gutter-md">
          <q-rating
            name="quality"
            v-model="quality"
            max="5"
            size="3.5em"
            color="accent"
            icon="star_border"
            icon-selected="star"
            no-dimming
          />

          <div class="q-mt-lg row justify-center">
            <q-btn no-caps label="Enviar" type="submit" color="primary"/>
          </div>
        </q-form>
      </div>
    </q-card>
  </q-dialog>
</template>

<script>
import { ref } from 'vue'

export default ({
  setup() {
    return {
      quality: ref(2),
      submitResult: ref([]),
      modalRating: ref(false)
    }
  },

  methods: {
    open() {
      this.modalRating = true
    },

    userExperience (evt) {
      const formData = new FormData(evt.target)
      const submitResult = []

      for (const [ name, value ] of formData.entries()) {
        submitResult.push({
          name,
          value
        })
      }

      this.submitResult = submitResult

      console.log(this.submitResult);

      this.modalRating = false

      this.$q.notify({
        position: "bottom",
        color: "secondary",
        textColor: "white",
        icon: "check_circle",
        message: 'Cadastro realizado com sucesso!',
      })
    }
  }
})
</script>
