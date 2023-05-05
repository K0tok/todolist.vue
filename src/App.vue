<template>
  <div class="app">
    <div class="foldersList">
      <div :class="[{ activeId: '-1' === activeId }, 'allFolder']" @click="activeId = '-1'; activeFolderId = '-1'">
        <img src="../images/menu.png" alt="" style="height: 30px;">
        <div style="margin-left: 10px">Все задачи</div>
      </div>
      <div v-if="folders.length === 0">
        Папок нет
      </div>
      <div v-else style="width: 100%; margin: 30px 10px 10px 10px;">
        <div 
        v-for="(folder, index) in folders" 
        :key="index" 
        class="folder"
        :class="[{ activeId: folder.id === activeId }, 'folder']"
        @click="activeId = folder.id; activeFolderId = folder.id"
        >
          <div class="circle" :class="folder.folderColor"></div>
          <div v-if="isEditFolder !== folder.id" style="margin-left: 10px">{{folder.folderName}}</div>
          <input v-else class="editInput" :placeholder="folder.folderName" v-model="activeEditFolderText">
          <img v-if="isEditFolder !== folder.id" src="../images/edit.png" alt="Edit" style="height: 35px; margin: 0 0 0 auto" @click="editFolder(folder.id)">
          <img v-else src="../images/tick.png" alt="Tick" style="height: 25px; margin: 0 0 0 auto" @click="acceptEditFolder(folder.id)">
          <img src="../images/x.png" alt="Delete" style="height: 20px; margin: 0 20px 0 10px" @click="deleteFolder(folder.id)">
        </div>
      </div>
      <div class="newFolder" @click="newFolder">
        + Добавить папку
      </div>
      <div class="idk" v-if="folderForm === false"></div>
      <div v-if="folderForm === true" class="folderForm">
        <img class="close" src="../images/close.png" @click="folderForm = false">
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
            <div v-if="IsEditTask !== task.id">{{task.text}}</div>
            <input v-else v-model="activeEditTaskText" :placeholder="task.text" class="editInput">
            <img v-if="IsEditTask !== task.id && activeFolderId !== '-1'" src="../images/edit.png" alt="Edit" style="height: 35px; margin: 0 0 0 20px; cursor: pointer;" @click="editTask(task.id)">
            <img v-else-if="IsEditTask == task.id && activeFolderId !== '-1'" src="../images/tick.png" alt="Tick" style="height: 25px; margin: 0 0 0 20px; cursor: pointer;" @click="acceptEditTask(task.id)">
            <img v-if="activeFolderId !== '-1'" src="../images/x.png" alt="Delete" style="height: 20px; margin: 0 20px 0 10px; cursor: pointer;" @click="deleteTask(task.id)">
          </div>
        </div>
        <div class="task taskFormButton" v-if="taskForm === false && activeId !== '-1'" @click="taskForm = true">+  Новая задача</div>
        <div class="task taskFormButton" v-if="taskForm === true && activeId !== '-1'" @click="taskForm = false">+  Новая задача</div>
        <div class="idk" v-if="taskForm === false"></div>
        <div class="task taskForm" v-if="taskForm === true && activeId !== '-1'">
          <img class="taskClose" src="../images/close.png" @click="taskForm = false">
          <input class="taskInput" v-model="activeText" placeholder="Текст Задачи">
          <button class="addTaskButton" @click="addTask(activeId)">Добавить</button>
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
            id: 1,
            folderName: 'testFolder',
            folderColor: 'pink',
            tasks: [
              {
                isDone: false,
                text: 'Закончить приложение',
                id: '0908787'
              },
              {
                isDone: true,
                text: 'Добавить Folder Form',
                id: '23113'
              },
              {
                isDone: false,
                text: 'Добавить Task Form',
                id: '423423'
              }
            ]
          }
        ],
        folderForm: false,
        taskForm: false,
        activeColor: 'red',
        activeName: '',
        activeId: '-1',
        activeFolderId: '-1',
        activeText: '',
        isEditFolder: '',
        activeEditFolderText: '',
        IsEditTask: '',
        activeEditTaskText: '',
        folderNumber: 1,
        colors: ['red', 'orange', 'pink', 'green', 'blue', 'purple', 'black']
      }
    },
    computed: {
      tasks() {
        if (this.activeFolderId === '-1') {
          return this.folders
        }
        else {
          let index = this.folders.findIndex(x => x.id === this.activeFolderId)
          return [this.folders[index]]
        }
      },
    },
    methods: {
      newFolder(){
        if (this.folderForm == true){
          this.folderForm = false
        }
        else{
          this.folderForm = true
        }
      },
      addFolder(){
        const folder = {
          id: new Date().getTime(),
          folderName: this.activeName,
          folderColor: this.activeColor,
          tasks: []
        }
        if (folder.folderName == ''){
          folder.folderName = 'Новая папка(' + this.folderNumber.toString() + ')'
          this.folderNumber += 1
        }
        this.folders.push(folder)
        this.activeColor = 'red'
        this.activeName = ''
        this.folderForm = false
      },
      addTask(activeId){
        const task = {
          isDone: false,
          text: this.activeText,
          id: new Date().getTime()
        }
        const index = this.folders.findIndex(x => x.id === activeId)
        this.folders[index].tasks.push(task)
        this.activeText = ''
        this.taskForm = false
      },
      deleteFolder(id){
        const index = this.folders.findIndex(x => x.id === id)
        this.folders.splice(index, 1)
        if (this.activeFolderId === id || this.folders.length ===0){
          this.activeFolderId = '-1'
          this.activeId = '-1'
        }
      },
      editFolder(id){
        this.isEditFolder = id
      },
      acceptEditFolder(id){
        const index = this.folders.findIndex(x => x.id === id)
        this.folders[index].folderName = this.activeEditFolderText
        this.isEditFolder = ''
        this.activeEditFolderText = ''
      },
      editTask(id){
        this.IsEditTask = id
      },
      acceptEditTask(id){
        const folderIndex = this.folders.findIndex(x => x.id === this.activeFolderId)
        const taskIndex = this.folders[folderIndex].tasks.findIndex(x => x.id === id)
        this.folders[folderIndex].tasks[taskIndex].text = this.activeEditTaskText
        this.IsEditTask = ''
        this.activeEditTaskText = ''
      },
      deleteTask(id){
        const folderIndex = this.folders.findIndex(x => x.id === this.activeFolderId)
        const taskIndex = this.folders[folderIndex].tasks.findIndex(x => x.id === id)
        this.folders[folderIndex].tasks.splice(taskIndex, 1)
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
  position: fixed;
  width: 35%;
  height: 100%;
  padding-top: 50px;
  background-color: #F4F6F8;
  border-right: 1px solid #F1F1F1;
  overflow: auto;

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
.activeId{
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
  margin: 30px 10px 10px -80px;
  padding: 5px;
  font-size: 30px;
  text-align: center;
  border-radius: 10px;
  cursor: pointer;
}
.editInput{
  width: 50%;
  height: 25px;
  font-size: 20px;
  margin-left: 10px
}
.folderForm{
  width: 90%;
  height: 150px;
  font-size: 30px;
  padding: 20px 0 20px 0;
  margin-bottom: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  background: #FFFFFF;
  box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
  border-radius: 10px;
  border-radius: 10px;
  z-index: 999;
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
.close{
  margin: -40px -15px 0px auto;
  height: 30px;
  width: 30px;
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
  width: 75%;
  height: 30px;
  font-size: 25px;
}
.folderTaskList{
  width: 65%;
  margin-left: 35%;
  padding: 100px 55px;
  background-color: #f4feff;
}
.taskList {
  display: flex;
  flex-direction: column;
}
.folderName{
  font-size: 50px;
  padding-bottom: 30px;
  margin-bottom: 20px;
  border-bottom: 2px solid #F2F2F2;
  background-color: white;
}
.task{
  padding-left: 30px;
  margin: 10px;
  font-size: 30px;
  display: flex;
  flex-direction: row;
  align-items: center;
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
.taskClose{
  margin: -30px -15px 0px auto;
  height: 30px;
  width: 30px;
}
.taskInput{
  width: 90%;
  height: 30px;
  font-size: 25px;
}
.addTaskButton{
  width: 91%;
  height: 40px;
  font-size: 25px;
  cursor: pointer;
  background: #4DD599;
  border: 1px solid #4DD599;
  border-radius: 4px;
}
.taskForm{
  width: 40%;
  height: 100px;
  padding: 10px;
  margin-bottom: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  background: #FFFFFF;
  border-radius: 4px;
  border-radius: 10px;
  border-radius: 10px;
  background-color: #F4F6F8;
}
.idk{
  height: 150px;
}
</style>
