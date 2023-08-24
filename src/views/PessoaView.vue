<template>
  <div>
    <v-btn @click.stop="dialog = true" elevation="1" color="primary">
      {{ newBtnTitle }}
    </v-btn>
    <TabelaComponent
      :headers="headers"
      :objects="pessoas"
      :itemsPerPage="itemsPerPage"
      @editar="editarPessoa"
      @excluir="excluirPessoa"
      @showDialog="dialog = true"
      @salvar="salvar"
    >
    </TabelaComponent>
    <FormComponent
      :titleNewObj="'Nova Pessoa'"
      :titleEditObj="'Editar Pessoa'"
      :showDialog="dialog"
      :titleBtnSalvar="'Save'"
      @clear-form="form = {}"
      :formObj="form"
      @disableDialog="dialog = false"
      @salvar="salvar"
      :rules="rules"
      :emailRules="emailRules"
      :phoneRules="phoneRules"
    >
    </FormComponent>

    <v-snackbar v-model="snackbar">
      {{ snackbarText }}
    </v-snackbar>
  </div>
</template>
<script>
import TabelaComponent from '@/components/TabelaComponent.vue'
import FormComponent from '@/components/FormComponent.vue'

export default {
  components: { TabelaComponent, FormComponent },
  data: function () {
    return {
      pessoas: [],
      snackbar: false,
      snackbarText: '',
      form: {},
      newBtnTitle: 'NOVA PESSOA',
      itemsPerPage: 5,
      dialog: false,
      rules: [(n) => !!n || 'Campo obrigatório'],
      emailRules: [
        (n) => !!n || 'Campo obrigatório',
        (n) => /.+@.+\..+/.test(n) || 'E-mail inválido',
      ],
      phoneRules: [
        (n) => !!n || 'Campo obrigatório',
        (n) =>
          /\([0-9]+\)\s[0-9]+-[0-9][0-9][0-9][0-9]/i.test(n) ||
          'Telefone inválido',
        (n) => n?.length == 15 || 'Telefone inválido',
      ],
      headers: [
        {
          text: 'Id',
          align: 'start',
          sortable: false,
          value: 'id',
        },
        { text: 'Nome', align: 'start', value: 'nome' },
        { text: 'Idade', align: 'start', value: 'idade' },
        { text: 'E-mail', align: 'start', value: 'email' },
        { text: 'Telefone', align: 'start', value: 'telefone' },
        { text: 'Cidade', align: 'start', value: 'cidade' },
        { value: 'editar', sortable: false, align: 'center' },
        { value: 'excluir', sortable: false, align: 'center' },
      ],
    }
  },
  methods: {
    toggleSnackbar(text) {
      this.snackbarText = text
      this.snackbar = true

      setTimeout(() => {
        this.snackbar = false
        this.snackbarText = ''
      }, 1000)
    },
    salvar(form) {
      const th = this

      if (form) this.form = form

      if (this.form.id) {
        th.$api.Pessoa.Put(this.form.id, this.form).then((response) => {
          this.toggleSnackbar('Registro editado com sucesso!')
        })
      } else {
        th.$api.Pessoa.Post(this.form).then((response) => {
          this.toggleSnackbar('Registro criado com sucesso!')
        })
      }

      this.form = {}

      this.dialog = false

      this.getPessoas()
    },
    getPessoas() {
      const th = this
      th.$api.Pessoa.GetAll().then(function (response) {
        th.pessoas = response
      })
    },
    excluirPessoa(item) {
      const th = this

      th.$api.Pessoa.Delete(item.id).then((response) => {
        console.log(response)
      })

      this.getPessoas()

      this.toggleSnackbar('Registro excluído com sucesso!')
    },
    editarPessoa(pessoa) {
      this.dialog = true

      this.form = pessoa
    },
  },
  mounted() {
    this.getPessoas()
  },
}
</script>
