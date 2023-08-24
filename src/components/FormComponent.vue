<template>
  <div>
    <v-dialog
      @click:outside="$emit('clearForm'), disableDialog()"
      v-model="dialog"
      width="600"
    >
      <div class="white">
        <v-form ref="form" @submit.prevent="$emit('salvar', form)">
          <v-card-title v-if="!form.id" class="text-h5">
            {{ titleNewObj }}
          </v-card-title>
          <v-card-title v-else class="text-h5">
            {{ titleEditObj }}
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="3">
                  <v-text-field
                    color="primary"
                    v-model="form.id"
                    label="Id"
                    :disabled="form.id ? true : false"
                    :class="[!form.id ? 'd-none' : '']"
                  >
                  </v-text-field>
                </v-col>
              </v-row>

              <v-row>
                <v-col cols="12" sm="12">
                  <v-text-field
                    color="primary"
                    v-model="form.nome"
                    label="Nome *"
                    :rules="rules"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="12" sm="12">
                  <v-slider
                    thumb-label="always"
                    v-model="form.idade"
                    label="Idade *"
                    max="200"
                    min="0"
                    :rules="rules"
                  ></v-slider>
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="12" sm="8">
                  <v-text-field
                    color="primary"
                    v-model="form.email"
                    label="E-mail *"
                    :rules="emailRules"
                  >
                  </v-text-field>
                </v-col>
                <v-col cols="12" sm="4">
                  <v-text-field
                    color="primary"
                    v-model="form.telefone"
                    placeholder="(99) 99999-9999"
                    label="Telefone *"
                    :rules="phoneRules"
                  >
                  </v-text-field>
                </v-col>
              </v-row>

              <v-row>
                <v-col>
                  <v-text-field
                    color="primary"
                    v-model="form.cidade"
                    label="Cidade *"
                    :rules="rules"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="primary"
              v-if="titleBtnFechar"
              @click="disableDialog(), $emit('clearForm')"
            >
              {{ titleBtnFechar }}
            </v-btn>
            <v-btn
              color="primary"
              v-else
              @click="
                disableDialog(), $emit('clearForm'), $emit('disableDialog')
              "
            >
              Fechar
            </v-btn>
            <v-btn type="submit" v-if="titleBtnSalvar" color="primary">
              {{ titleBtnSalvar }}
            </v-btn>
            <v-btn type="submit" v-else color="primary"> Salvar </v-btn>
          </v-card-actions>
        </v-form>
      </div>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: 'FormComponent',
  emits: ['clearForm', 'salvar', 'disableDialog'],
  props: [
    'titleCase1',
    'titleCase2',
    'showDialog',
    'titleBtnSalvar',
    'titleBtnFechar',
    'formObj',
    'titleEditObj',
    'titleNewObj',
    'rules',
    'phoneRules',
    'emailRules',
  ],
  data() {
    return {
      dialog: this.showDialog,
      form: this.formObj,
    }
  },
  methods: {
    disableDialog() {
      this.dialog = false

      this.$emit('disableDialog')

      this.form = {}
    },
  },
  watch: {
    showDialog: function (newVal, oldVal) {
      this.dialog = newVal
    },
    formObj: function (newVal, oldVal) {
      this.form = newVal
    },
  },
}
</script>
