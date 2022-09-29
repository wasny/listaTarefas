<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <h1>Tarefas</h1>
      </div>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row class="text-center">
          <v-col offset-lg="2" lg="8" md="12">
            <v-row>
              <v-col cols="12">
                <v-text-field v-model="tarefa.descricao" label="Descrição"
                  placeholder="Digite o item de sua Lista de Tarefas"
                  title="Digite aqui o item de sua Lista de Tarefas"></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" class="text-right">
                <v-btn class="primary" @click="salvarTarefa">Salvar</v-btn>
                <v-btn class="red" onclick="excluir()" title="Clique para excluir o item desta lista">
                  Excluir</v-btn>
                <v-btn class="orange" @click="limpaTarefa"
                  title="Clique para limpar as informações, sem efetuar mudanças">Limpar</v-btn>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
        <v-row class="text-center lighten-4 blue">
          <v-col offset-lg="2" lg="8" md="12">
            <v-simple-table>
              <thead>
                <tr>
                  <th class="text-left" style="width: 75%">Descrição</th>
                  <th class="text-left">Ação</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(tarefa, indice) in tarefas" :key="indice" :class="tarefa.feita ? 'lighten-4 orange' : ''">
                  <td class="text-left" @click="seleciona(indice)">{{ tarefa.descricao }}</td>
                  <td class="text-left">
                    <v-btn class="blue lighten-1 white--text" v-if="!tarefa.feita" @click="alteraStatusTarefa(tarefa)">
                      <v-icon left> mdi-clipboard-text-off-outline </v-icon>Feito
                    </v-btn>
                    <v-btn class="red lighten-1 white--text" v-else @click="alteraStatusTarefa(tarefa)">
                      <v-icon left> mdi-clipboard-text-outline </v-icon> A
                      fazer
                    </v-btn>
                  </td>
                </tr>
              </tbody>
            </v-simple-table>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>
 
<script>
export default {
  name: "App",

  components: {},

  data: () => ({
    tarefa: {
      descricao: "",
      feita: false,
    },
    tarefas: [],
    indice: -1,
  }),

  methods: {

    addLocal() {
      window.localStorage.setItem('listaTarefas', JSON.stringify(this.tarefas));
    },

    readLista() {
      var listaDisc = JSON.parse(window.localStorage.getItem('listaTarefas'));
      this.tarefas = (listaDisc != null) ? listaDisc : [];
    },

    limpaTarefa() {
      this.tarefa = {
        descricao: "",
        feita: false,
      };
    },

    salvarTarefa() {
      if ((this.indice < 0) && (this.tarefa.descricao != null)) {
        this.tarefas.push(this.tarefa);
      } else {
        this.tarefas.splice(this.indice, 1, this.tarefa);
      }
      this.addLocal();
      this.limpaTarefa();

    },

    alteraStatusTarefa(t) {
      t.feita = !t.feita;
      this.addLocal();
    },

    seleciona(indice) {
      let listaTarefas = JSON.parse(window.localStorage.getItem('listaTarefas'));
      this.tarefa = listaTarefas[indice];
    },
  },

  beforeMount() {
    this.readLista();
  },

};
</script>
