<template>
  <v-app>
    <v-container class="container">
      <v-row>
        <v-col cols="12">
          <v-alert type="success" v-if="addSuccess">Tache ajouté avec succes</v-alert>
          <v-alert type="info" v-if="removeSuccess">Tache supprimé avec succes</v-alert>
          <v-alert type="warning" v-if="confirmSuccess">Tache terminé !</v-alert>
          <v-alert type="success" v-if="reAddTache">Tache réstauré !</v-alert>
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
                    <v-btn small color="warning" class="mr-2" @click="showOldList">
                      <i class="fas fa-ellipsis-h"></i>
                    </v-btn>
                    <v-btn small color="primary" @click="enableAddBloc">
                      <i class="fa fa-plus"></i>
                    </v-btn>
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(el, index) in list" :key="index">
                  <td class="text-left">{{ el }}</td>
                  <td class="text-right">
                    <v-btn small color="success" class="mr-2" @click="confirmTache(index)">
                      <i class="fa fa-check"></i>
                    </v-btn>
                    <v-btn small color="error" @click="removeTache(index)">
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

      <v-simple-table v-if="oldListSuccess">
        <thead>
          <tr>
            <th>Tâches terminées</th>
            <th class="text-right">
              <v-btn small color="error" @click="closeOldTache">
                <i class="fa fa-times"></i>
              </v-btn>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(el, index) in oldList" :key="index">
            <td class="text-left">
              <s>{{ el }}</s>
            </td>
            <td class="text-right">
              <v-btn small color="warning" @click="restoreTache(index)">
                <i class="fas fa-level-up-alt"></i>
              </v-btn>
            </td>
          </tr>
        </tbody>
      </v-simple-table>

      <v-row class="text-center">
        <v-col>ToDoList - PiersantiDylan</v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
const list = JSON.parse(localStorage.getItem("list")) || [];

export default {
  name: "App",

  data: () => ({
    list,
    oldList: [],
    show: false,
    addSuccess: false,
    removeSuccess: false,
    confirmSuccess: false,
    oldListSuccess: false,
    reAddTache: false
  }),
  create() {
    localStorage.setItem("list", JSON.stringify(this.list));
  },
  methods: {
    enableAddBloc: function() {
      this.show = true;
    },

    disableAddBloc: function() {
      this.show = false;
    },

    addTache: function() {
      let newTache = document.getElementById("setTache");
      this.list.push(newTache.value);
      newTache.value = "";
      this.addSuccess = true;
      this.removeSuccess = false;
      this.confirmSuccess = false;
      this.reAddTache = false;
      localStorage.setItem("list", JSON.stringify(this.list));
    },

    removeTache: function(index) {
      this.list.splice(index, 1);
      this.removeSuccess = true;
      this.addSuccess = false;
      this.confirmSuccess = false;
      this.reAddTache = false;
    },

    confirmTache: function(index) {
      this.oldList.push(this.list[index]);
      this.list.splice(index, 1);
      this.confirmSuccess = true;
      this.removeSuccess = false;
      this.addSuccess = false;
      this.reAddTache = false;
    },

    restoreTache: function(index) {
      this.list.push(this.oldList[index]);
      this.oldList.splice(index, 1);
      this.confirmSuccess = false;
      this.removeSuccess = false;
      this.addSuccess = false;
      this.reAddTache = true;
    },

    showOldList: function() {
      this.oldListSuccess = true;
    },

    closeOldTache: function() {
      this.oldListSuccess = false;
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