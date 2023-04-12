<template>
  <div class="app">
    <div class="foldersList">
      <div 
      v-for="(folder, index) in folders" 
      :key="index" 
      class="folder" 
      >
        <div>

        </div>
      </div>
      <div class="folder" @click="newFolder">
        + Добавить папку
      </div>
      <div v-if="folderForm === true" class="folderForm">
        <input v-model="folder.folderName" style="width: 80%; ">

        <div class="circles">
          <div class="circle red" :class="{'active': activeColor === 'red'}" @click="activeColor = 'red'"></div>
          <div class="circle orange"></div>
          <div class="circle pink"></div>
          <div class="circle green"></div>
          <div class="circle blue"></div>
          <div class="circle purple"></div>
          <div class="circle black"></div>
        </div>
        <button class="addFolderButton" @click="addFolder">Добавить</button>
      </div>
    </div>
    <div class="taskList">
        <!-- <div v-if="tasks.length === 0">
            Задачи отсутствуют
        </div> -->
    </div>
  </div>
</template>

<script>

  export default {
    name: 'App',
    data() {
      return {
        folders: [],
        folderForm: false,
        folder: {
          folderName: '',
          folderColor: '',
          tasks: [{

          }]
        },
        activeColor: '',
      }
    },
    methods: {
      newFolder(){
        this.folderForm = true
      },
      addFolder(){
        this.folders.push(this.folder)
        this.folderName = ''
        this.folderColor = ''
        this.folderForm = false
      }
    }
  }
</script>

<style>
body{
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
}
.app{
  height: 100%;
  background-color: white;

  display: flex;
  flex-direction: row;
}
.foldersList {
  width: 30%;
  height: 100%;
  background-color: #F4F6F8;
  border-right: 1px solid #F1F1F1;

  display: flex;
  flex-direction: column;
  align-items: center;
}
.folder{
  width: 80%;
  margin: 10px;
  padding: 5px;
  font-size: 30px;
  text-align: center;
  border: 3px solid black;
  border-radius: 10px;
}
.folderForm{
  width: 80%;
  height: 100px;
  font-size: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 3px solid black;
  border-radius: 10px;
}
.circles{
  display: flex;
  width: 80%;
  justify-content: space-between;
}
.circle{
  width: 30px; 
  height: 30px; 
  border-radius: 50%; 
  border: 1px solid rgb(104, 104, 104);
}
.red{
  background-color: rgb(255, 70, 70); 
}
.green{
  background-color: rgb(84, 179, 84); 
}
.orange{
  background-color: rgb(241, 176, 55); 
}
.pink{
  background-color: rgb(255, 136, 156); 
}
.blue{
  background-color: rgb(81, 81, 255); 
}
.purple{
  background-color: rgb(168, 63, 168); 
}
.black{
  background-color: black; 
}
.active {
  border: 1px solid black;
}
</style>
