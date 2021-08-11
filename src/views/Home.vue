<template>
  <div>
    <v-row>
      <v-col cols="auto">
        <v-combobox
          v-model="categoriasFiltrar"
          label="Filtrar"
          :items="categorias"
          multiple
          outlined
        ></v-combobox>
      </v-col>
      <v-col cols="auto">
        <v-btn-toggle v-model="toggleVista">
          <v-btn>
            <v-icon>mdi-view-dashboard</v-icon>
          </v-btn>

          <v-btn>
            <v-icon>mdi-table</v-icon>
          </v-btn>
        </v-btn-toggle>
      </v-col>
    </v-row>
    <v-row v-if="toggleVista == 0">
      <v-col
        cols="12"
        sm="6"
        md="4"
        lg="3"
        v-for="(alimento, index) in getAlimentos"
        :key="index"
      >
        <v-card @click="onClickAlimento(alimento)">
          <v-img height="200" :src="alimento.cover"></v-img>
          <v-card-title> {{ alimento.nombre }} </v-card-title>
          <v-card-subtitle> {{ alimento.cantidad }} </v-card-subtitle>
          <v-card-text>
            <v-chip small color="success" outlined>
              {{ getNombreCategoria(alimento.categoria) }}
            </v-chip>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-if="toggleVista == 1">
      <v-col cols="12">
        <v-data-table :headers="headers" :items="getAlimentos"></v-data-table>
      </v-col>
    </v-row>
    <v-dialog v-model="dialogoDetalle" width="800">
      <v-card elevation="5">
        <v-carousel height="300" hide-delimiters v-if="alimentoDetail.imagenes">
          <v-carousel-item
            v-for="(item, i) in alimentoDetail.imagenes"
            :key="i"
            :src="item"
          ></v-carousel-item>
        </v-carousel>
        <v-img v-else height="200" :src="alimentoDetail.cover"></v-img>
        <v-card-title>
          {{ alimentoDetail.nombre }}
        </v-card-title>
        <v-card-subtitle> {{ alimentoDetail.cantidad }} </v-card-subtitle>

        <v-card-text>
          {{ alimentoDetail.descripcion }}
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red" text @click="dialogoDetalle = false">
            Cerrar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import alimentos from "@/utils/alimentos";

const CATEGORIA_ALIMENTO = {
  verduras: "Verduras",
  frutas: "Frutas",
  cerealesA: "Cereales y Tuberculos sin grasa (A)",
  cerealesB: "Cereales y Tuberculos con grasa (B)",
  leguminosas: "leguminosas",
  animal: "Alimentos de origen animal",
  leche: "Leche",
  aceitesA: "Aceites y grasas (A)",
  aceitesB: "Aceites y grasas con Proteina (B)",
  azucares: "Azucares",
  sinEnergia: "Alimentos libres de energía",
};

export default {
  name: "Home",

  components: {},
  data() {
    return {
      dialogoDetalle: false,
      toggleVista: 0,
      alimentos: alimentos,
      categoriasFiltrar: [],
      categorias: Object.keys(CATEGORIA_ALIMENTO),
      headers: [
        {
          text: "Alimento",
          value: "nombre",
          align: "start",
          sortable: false,
        },
        {
          text: "Cantidad",
          value: "cantidad",
          align: "start",
          sortable: false,
        },
        {
          text: "Categoria",
          align: "start",
          sortable: true,
          value: "categoria",
        },
      ],
      alimentoDetail: {
        nombre: "",
        cantidad: "",
        categoria: "",
        cover: "",
        descripcion: "",
      },
    };
  },
  methods: {
    getNombreCategoria(categoria) {
      return CATEGORIA_ALIMENTO[categoria];
    },
    onClickAlimento(alimento) {
      this.alimentoDetail = { ...alimento };
      this.dialogoDetalle = true;
    },
  },
  computed: {
    getAlimentos() {
      if (this.categoriasFiltrar.length > 0) {
        return this.alimentos.filter((element) => {
          if (this.categoriasFiltrar.includes(element.categoria)) {
            return element;
          }
        });
      } else {
        return this.alimentos;
      }
    },
  },
};
</script>
