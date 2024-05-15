<template>
  <div class="container">
    <HeaderComp :searchTasks="searchTasks" :sortBy="sortBy" 
    @update:sortBy="sortBy = $event"
    @update:searchTasks="searchTasks=$event" @show-modal="showModal"/>
    <MainComp :tasks="tasks"  @hide-modal="hideModal" :isModalVisible="isModalVisible" :textValue="textValue"
      @change-status="changeStatus" @add-task="addTask" 
      @update:textValue="textValue=$event"
      :searchTasks="searchTasks"
     />
  </div>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue'
import MainComp from './components/MainComp.vue'

   export default {
      components:{
        HeaderComp,MainComp
      },
      data(){
        return{
          textValue: '',
          tasks:[],
          isModalVisible:false,
          sortBy: 'date',
          searchTasks:''
        }
      },
      created(){
        this.loadStorage()
      },
      computed:{
        filterTasks(){
          return this.tasks.filter(task=>{
            return(
              task.name.toLowerCase().includes(this.searchTasks.toLowerCase()) ||
              task.status.toLowerCase().includes(this.searchTasks.toLowerCase()) ||
              task.date.toLowerCase().includes(this.searchTasks.toLowerCase())
            )
          })
        }
      },
      
      
      methods:{
        sort(){
          if(this.sortBy === 'date'){
            this.tasks.sort((a,b)=> new Date(a.date) - new Date(b.date))
          }else if(this.sortBy === 'status'){
            this.tasks.sort((a,b)=> a.status.localeCompare(b.status))
          }
        },
        changeStatus(index){
          this.tasks[index].status = this.tasks[index].isChecked ? 'Выполнено' : 'В работе'
          this.storageSave()
        },
        addTask(){
          if(this.textValue.trim() === ''){
       
            return
          }else{
            this.tasks.push({
              name:this.capitalizeFirstLetter(this.textValue),
              isChecked:false,
              status:'В работе',
              date:new Date().toLocaleDateString()
            })
            this.textValue = ''
            console.log(this.tasks);
            this.isModalVisible = false
            this.storageSave()
          }
        },
        showModal(){
          this.isModalVisible = true
        },
        hideModal(){
          this.isModalVisible = false
        },
        capitalizeFirstLetter: function(string){
          return string.charAt(0).toUpperCase() + string.slice(1)
        },
        storageSave(){
          localStorage.setItem('tasks',JSON.stringify(this.tasks))
        },
        loadStorage(){
          const tasks = localStorage.getItem('tasks')
          if(tasks){
            this.tasks = JSON.parse(tasks)
          }
        }
      }
   
   }
</script>

<style lang="scss">
@import url(https://db.onlinewebfonts.com/c/d3085f686df272f9e1a267cc69b2d24f?family=Montserrat-Bold);
@import url(https://db.onlinewebfonts.com/c/1d81cdaee5e55acbcf50ccbfc8b94122?family=Vela+Sans);
@import url(https://db.onlinewebfonts.com/c/101256d71b969fdd74e50f9f556e352c?family=AGAvantGardeCyr+Book);




/* Обнуляющие стили */
* {
	padding: 0px;
	margin: 0px;
	border: none;
}

*,
*::before,
*::after {
	box-sizing: border-box;
}

/* Links */

a, a:link, a:visited  {
    text-decoration: none;
}

a:hover  {
    text-decoration: none;
}


aside, nav, footer, header, section, main {
	display: block;
}

ul, ul li {
	list-style: none;
}

img {
	vertical-align: top;
}

img, svg {
	max-width: 100%;
	height: auto;
}

address {
  font-style: normal;
}

/* Form */

input, textarea, button, select {
	font-family: inherit;
    font-size: inherit;
    color: inherit;
    background-color: transparent;
}

input::-ms-clear {
	display: none;
}

button, input[type="submit"] {
    display: inline-block;
    box-shadow: none;
    background-color: transparent;
    background: none;
    cursor: pointer;
}

input:focus, input:active,
button:focus, button:active {
    outline: none;
}

button::-moz-focus-inner {
	padding: 0;
	border: 0;
}

label {
	cursor: pointer;
}

legend {
	display: block;
}





/* Общие стили */

body{
  background-color:white;  
}

#app{
 font-family: 'Vela Sans';
 font-weight: 400;
 font-size: 14px;
}

main{
  margin-top:40px;
}


h1{
  font-family: 'Montserrat-Bold';
  color:#16191D;
  font-size: 24px;
  line-height: 1.32;
}

.container{
  max-width:1300px;
  margin: 0 auto;

}

.todo-app{
  cursor: pointer;
}


</style>
