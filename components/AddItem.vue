<template>
  <v-dialog v-model="dialogKey" max-width="500px">
    <v-card>
      <v-card-title>
        <span class="headline">{{ formTitle }}</span>
      </v-card-title>

      <v-card-text>
        <v-container>
          <v-row>
            <v-col
              v-for="header in headers"
              :key="header.id"
              cols="12"
              sm="6"
              md="4"
            >
              <v-text-field
                v-model="form.name"
                :label="header.name"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="close"> Cancelar </v-btn>
        <v-btn color="blue darken-1" text @click="save"> Guardar </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: 'AddItem',
  props: {
    dialog: { type: Boolean, default: null },
  },
  data() {
    return {
      dialogKey: this.dialog,
      form: {},
    }
  },
}
</script>
