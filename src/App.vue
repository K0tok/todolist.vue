<template>
  <div class="app">
    <div class="foldersList">
      <div :class="[{ activeFolder: 'allFolders' === activeFolder }, 'allFolder']" @click="activeFolder = 'allFolders'">
        <!-- <img src="../images/menu.png" alt=""> -->
        <div style="margin-left: 10px">Все задачи</div>
      </div>
      <div 
      v-for="(folder, index) in folders" 
      :key="index" 
      class="folder"
      :class="[{ activeFolder: folder.folderName === activeFolder }, 'folder']"
      @click="activeFolder = folder.folderName; activeFolderIndex = index"
      >
        <div class="circle" :class="folder.folderColor"></div>
        <div style="margin-left: 10px">{{folder.folderName}}</div>
      </div>
      <div class="newFolder" @click="newFolder">
        + Добавить папку
      </div>
      <div v-if="folderForm === true" class="folderForm">
        <input v-model="activeName" class="folderInput">

        <div class="circles">
          <div 
          v-for="(color, index) in colors"
          :key="index"
          :class="[{ active: color === activeColor}, 'circle', color]" @click="activeColor = color"></div>
        </div>
        <button class="addFolderButton" @click="addFolder">Добавить</button>
      </div>
    </div>
    <div 
    v-for="(taskList, index) in tasks"
    :key="index"
    class="taskList"
    >
      <div
      v-for="(task, index) in taskList"
      :key="index"
      class="taskFolder"
      >
        <div class="task">
          <input type="checkbox">
          <div>{{task.text}}</div>
        </div>
      </div>
    </div>
      <!-- <div v-if="tasks.length === 0">
          Задачи отсутствуют
      </div> -->
  </div>
</template>

<script>

  export default {
    name: 'App',
    data() {
      return {
        folders: [
          {
            folderName: 'testFolder',
            folderColor: 'pink',
            tasks: [
              {
                isDone: false,
                text: 'Закончить приложение'
              },
              {
                isDone: true,
                text: 'Добавить Folder Form'
              },
              {
                isDone: false,
                text: 'Добавить Task Form'
              }
            ]
          }
        ],
        folderForm: false,
        activeColor: 'red',
        activeName: '',
        activeFolder: 'allFolders',
        activeFolderIndex: '',
        colors: ['red', 'orange', 'pink', 'green', 'blue', 'purple', 'black']
      }
    },
    computed: {
      tasks() {
        if (this.activeFolderIndex === '') {
          return this.folders
        }
        else {
          return [this.folders[this.activeFolderIndex]]
        }
      },
    },
    methods: {
      newFolder(){
        this.folderForm = true
      },
      addFolder(){
        const folder = {
          folderName: this.activeName,
          folderColor: this.activeColor,
          tasks: [{

          }]
        }
        this.folders.push(folder)
        this.activeColor = 'red'
        this.activeName = ''
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
  border-radius: 10px;
  cursor: pointer;
  display: flex;
  align-items: center;
  padding-left: 10%;
}
.allFolder{
  width: 80%;
  margin: 10px;
  padding: 5px;
  font-size: 30px;
  text-align: center;
  border-radius: 10px;
  cursor: pointer;
  display: flex;
  align-items: center;
  padding-left: 10%;
  padding-top: 20px;
  padding-bottom: 20px;
}
.activeFolder{
  width: 80%;
  margin: 10px;
  padding: 5px;
  font-size: 30px;
  text-align: center;
  background: #FFFFFF;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
  border-radius: 10px;
  cursor: pointer;
  display: flex;
  align-items: center;
  padding-left: 10%;
  padding-top: 20px;
  padding-bottom: 20px;
}
.newFolder{
  width: 80%;
  margin: 10px;
  padding: 5px;
  font-size: 30px;
  text-align: center;
  border-radius: 10px;
  cursor: pointer;
}
.folderForm{
  width: 80%;
  height: 100px;
  font-size: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
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
  border: 2px solid rgb(68, 68, 68);
}
.folderInput{
  width: 80%;
  height: 30px;
  font-size: 25px;
}
.taskList {
  width: 70%;
  padding: 56px 55px;
  display: flex;
  flex-direction: column;
  border: 1px solid black;  
}
</style>
