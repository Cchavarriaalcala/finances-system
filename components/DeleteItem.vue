<template>
  <v-dialog v-model="dialogDeleteKey" max-width="500px">
    <v-card>
      <v-card-title class="headline">
        Estás seguro de eliminar este elemento?
      </v-card-title>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="closeDelete">Cancelar</v-btn>
        <v-btn color="blue darken-1" text @click="deleteItemConfirm"
          >Confirmar</v-btn
        >
        <v-spacer></v-spacer>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: 'DeleteItem',
  props: {
    dialogDelete: { type: Boolean, default: null },
  },
  data() {
    return {
      dialogDeleteKey: this.dialogDelete,
    }
  },
}
</script>
