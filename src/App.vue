<template>
  <v-app>
    <v-container class="container">
      <v-row>
        <v-col cols="12">
          <v-alert type="success" v-if="addSuccess">Tache ajouté avec succes</v-alert>
          <v-alert type="info" v-if="removeSuccess">Tache supprimé avec succes</v-alert>
          <v-alert type="warning" v-if="confirmSuccess">Tache terminé !</v-alert>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12">
          <v-card>
            <v-simple-table>
              <thead>
                <tr>
                  <th>Tâches en cours</th>
                  <th class="text-right">
                    <v-btn small color="warning" class="mr-2" @click="enableAddBloc">
                      <i class="fas fa-ellipsis-h"></i>
                    </v-btn>
                    <v-btn small color="primary" @click="enableAddBloc">
                      <i class="fa fa-plus"></i>
                    </v-btn>
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="e in list" :key="e">
                  <td class="text-left">{{ e }}</td>
                  <td class="text-right">
                    <v-btn small color="success" class="mr-2" @click="confirmTache(e)">
                      <i class="fa fa-check"></i>
                    </v-btn>
                    <v-btn small color="warning" class="mr-2">
                      <i class="fa fa-cog"></i>
                    </v-btn>
                    <v-btn small color="error" @click="removeTache(e)">
                      <i class="fas fa-trash"></i>
                    </v-btn>
                  </td>
                </tr>
              </tbody>
            </v-simple-table>
          </v-card>
          <v-card class="mt-3">
            <v-container v-if="show">
              <v-row>
                <v-col cols="8">
                  <input type="text" class="addBloc" id="setTache" />
                </v-col>
                <v-col cols="4">
                  <v-btn class="text-right" color="primary" @click="addTache">Add</v-btn>
                  <v-btn class="text-right ml-2" color="error" @click="disableAddBloc">
                    <i class="fa fa-times"></i>
                  </v-btn>
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-col>
      </v-row>

      <v-card>
        <v-simple-table>
          <thead>
            <tr>
              <th>Tâches en cours</th>
            </tr>
          </thead>
          <tbody v-if="oldListSuccess">
            <tr v-for="el in oldList" :key="el">
              <td class="text-left">{{ e }}</td>
              <td class="text-right">
                <v-btn small color="success" class="mr-2" @click="confirmTache(e)">
                  <i class="fa fa-check"></i>
                </v-btn>
                <v-btn small color="warning" class="mr-2">
                  <i class="fa fa-cog"></i>
                </v-btn>
                <v-btn small color="error" @click="removeTache(e)">
                  <i class="fas fa-trash"></i>
                </v-btn>
              </td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-card>

      <v-row class="text-center">
        <v-col>ToDoList - PiersantiDylan</v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    list: [],
    oldList: [],
    show: false,
    addSuccess: false,
    removeSuccess: false,
    confirmSuccess: false,
    oldListSuccess: false
  }),

  methods: {
    enableAddBloc: function() {
      this.show = true;
    },

    disableAddBloc: function() {
      this.show = false;
    },

    addTache: function() {
      let newTache = document.getElementById("setTache").value;
      this.list.push(newTache);
      this.addSuccess = true;
      this.removeSuccess = false;
    },

    removeTache: function(e) {
      this.list.splice(e, 1);
      this.removeSuccess = true;
      this.addSuccess = false;
    },

    confirmTache: function(e) {
      this.oldList.push(this.list[e]);
      this.list.splice(e, 1);
      this.confirmSuccess = true;
      this.addSuccess = false;
    }
  }
};
</script>

<style scoped>
.container {
  width: 600px;
}

.addBloc {
  width: 100%;
  height: 35px;
  padding: 10px;
  border: 1px dotted #131517;
  border-radius: 5px;
}
</style>