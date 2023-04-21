<template>
  <div class="app">
    <div class="foldersList">
      <div :class="[{ activeFolder: 'allFolders' === activeFolder }, 'allFolder']" @click="activeFolder = 'allFolders'">
        <img src="../images/menu.png" alt="" style="height: 30px;">
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
        <img src="../images/x.png" alt="Delete" style="height: 25px; margin-left: auto" @click="deleteFolder(index)">
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
    <div class="folderTaskList">
      <div
      v-if="folders.length === 0"
      style="width: 100%; font-size: 50px; text-align: center; color: grey;">
        Задачи отсутствуют
      </div>
      <div 
      v-for="(taskList, index) in tasks"
      :key="index"
      class="taskList"
      >
        <div :class="taskList.folderColor" class="folderName"><b>{{taskList.folderName}}</b></div>
        <div
        v-for="(task, index) in taskList.tasks"
        :key="index"
        class="taskFolder"
        >
          <div class="task">
            <input class="taskCheck" type="checkbox" v-model="task.isDone">
            <div>{{task.text}}</div>
          </div>
        </div>
        <div class="task taskFormButton" v-if="taskForm === false && activeFolder !== 'allFolders'" @click="taskForm = true">+  Новая задача</div>
        <div class="task taskFormButton" v-if="taskForm === true && activeFolder !== 'allFolders'" @click="taskForm = false">+  Новая задача</div>
        <div class="task taskForm" v-if="taskForm === true && activeFolder !== 'allFolders'">
          <input class="taskText" v-model="activeText" placeholder="Текст Задачи">
          <button class="addFolderButton" @click="addTask(activeFolder)">Добавить</button>
        </div>
      </div>
    </div>
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
        taskForm: false,
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
      },
      addTask(folderName){
        const task = {
          isDone: false,
          text: this.activeText
        }
        const index = this.folders.findIndex(x => x.folderName === folderName)
        this.folders[index].tasks.push(task)
        this.activeText = ''
        this.taskForm = false
      },
      deleteFolder(index){
        this.folders.pop(index)
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
  padding-top: 50px;
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
  width: 90%;
  height: 150px;
  font-size: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  background: #FFFFFF;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
  border-radius: 10px;
  border-radius: 10px;
}
.addFolderButton{
  width: 80%;
  height: 40px;
  font-size: 25px;
  cursor: pointer;
  background: #4DD599;
  border: 1px solid #4DD599;
  border-radius: 4px;
}
.circles{
  display: flex;
  width: 80%;
  justify-content: space-between;
  cursor: pointer;
}
.circle{
  width: 30px; 
  height: 30px; 
  border-radius: 50%; 
}
.red{
  background-color: rgb(255, 70, 70); 
  color: rgb(255, 70, 70); 
}
.green{
  background-color: rgb(84, 179, 84); 
  color: rgb(84, 179, 84); 
}
.orange{
  background-color: rgb(241, 176, 55); 
  color: rgb(241, 176, 55); 
}
.pink{
  background-color: rgb(255, 136, 156); 
  color: rgb(255, 136, 156); 
}
.blue{
  background-color: rgb(81, 81, 255); 
  color: rgb(81, 81, 255); 
}
.purple{
  background-color: rgb(168, 63, 168); 
  color: rgb(168, 63, 168); 
}
.black{
  background-color: black; 
  color: black; 
}
.active {
  border: 2px solid rgb(68, 68, 68);
}
.folderInput{
  width: 80%;
  height: 30px;
  font-size: 25px;
}
.folderTaskList{
  width: 70%;
  padding: 100px 55px;
}
.taskList {
  display: flex;
  flex-direction: column;
}
.folderName{
  font-size: 50px;
  background-color: white;
}
.task{
  padding-left: 30px;
  margin: 10px;
  font-size: 20px;
  display: flex;
  flex-direction: row;
}
.taskCheck{
  width: 20px;
  margin-right: 10px;
}
.taskFormButton{
  margin: 0;
  padding-left: 45px;
  font-size: 30px;
  color: grey;
  cursor: pointer;
}
.taskForm{
  width: 30%;
  height: 70px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  background: #FFFFFF;
  border-radius: 4px;
  border-radius: 10px;
  border-radius: 10px;

}
</style>
