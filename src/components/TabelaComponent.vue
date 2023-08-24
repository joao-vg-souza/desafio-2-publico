<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="objects"
      :items-per-page="itemsPerPage"
      class="elevation-1"
    >
      <template v-slot:item.editar="{ item }">
        <v-btn
          @click="
            $emit('editar', item),
              toggleSnackbar('Registro editado com sucesso')
          "
          color="primary"
          >Editar</v-btn
        >
      </template>
      <template v-slot:item.excluir="{ item }">
        <v-btn
          @click="
            $emit('excluir', item),
              toggleSnackbar('Registro excluído com sucesso')
          "
          color="error"
          >Excluir</v-btn
        >
      </template>
    </v-data-table>
  </div>
</template>

<script>
export default {
  name: 'TabelaComponent',
  emits: ['editar', 'excluir'],
  props: ['headers', 'objects', 'itemsPerPage', 'newBtnTitle'],
}
</script>
