<template >
    <main class="main">
      <div class="todo-content">
        <table class="table">
          <thead>
            <tr>
              <th></th>
              <th class="border">Описание</th>
              <th class="border">Статус</th>
              <th class="border">Дата</th>
            </tr>
          </thead>

          <tbody>
            <tr class="line" v-for="(task,index) in filterTasks" :key="index" :style="{background:task.isChecked ? 'rgb(246, 249, 255)' : 'white',boxShadow:task.isChecked ? '0px 4px 4px 0px rgba(0, 0, 0, 0.25)' : 'none', borderTop: '3px solid gray'}">
              <td class="elem"><input :id="`checkbox-${index}`" type="checkbox" v-model="task.isChecked" @change="$emit('change-status',index)">
              <label :for="`checkbox-${index}`"></label></td>
              <td class="elem">{{task.name}}</td>
              <td class="elem elem-status" :style="{color:task.isChecked ? '#134EC1' : '#F89B11'}">{{task.status}}</td>
              <td class="elem">{{task.date}}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="todo-modal" v-if="isModalVisible">
        <div class="todo-modal__title">
          <h2>Создать новую задачу</h2>
          <button @click="$emit('hide-modal')"><img src="img/button-modal.svg" alt=""></button>
        </div>
        
        <div class="todo-modal__main">
          <span>Описание</span>
          <input class="input-add" placeholder="Введите описание"  :value="textValue" @input="$emit('update:textValue',$event.target.value)"/>
          <button @click="$emit('addTask')">Создать</button>
        </div>
        
      </div>
      <div class="overlay" v-if="isModalVisible"></div>
    </main>
</template>
<script>
export default {
    props:{
        tasks:{
            type:Array,
            default:()=> []
        },
        isModalVisible:{
            type:Boolean,
            default:false
        },
        textValue:{
            type:String,
            default:''
        },
        searchTasks:{
            type:String,
            default:''
        }
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
        addTask(){
            if(this.textValue.trim() === ''){
                return
            }else{
                this.$emit('add-task')
            }
        }
    }
}
</script>
<style lang="scss">
    .table th,
.table td{
  padding:10px;
  padding-left:20px;
  text-align: left;
  
}




.table thead{
  height: 30px;
}
.table th:first-child,
.table td:first-child{
  min-width:80px;
  
}

.table td:first-child{
  display:flex;
  justify-content: center;
}

.table th:nth-child(2),
.table td:nth-child(2){
  width:100%;
  flex:1 0 auto;
}


.table th:nth-child(3),
.table td:nth-child(3),
.table th:nth-child(4),
.table td:nth-child(4){
  min-width:151px;
}

.todo-modal{
  position:fixed;
  top:50%;
  left:50%;
  transform:translate(-50%,-50%);
  z-index: 20;
  border-radius: 6px;
  width:400px;
  height: 281px;
  border: 1px solid #DDE2E4;
  padding:40px;
  box-shadow: 0px 25px 50px -12px rgba(0, 0, 0, 0.25);

  &__title{
    display:flex;
    justify-content: space-between;
    margin-bottom: 30px;
    h2{
       font-family: 'Montserrat-Bold',sans-serif;
       font-size: 18px;
    }
  }
}

.todo-modal__main{
  display:flex;
  flex-direction: column;
  span{
    margin-bottom: 5px;
    font-family: 'AGAvantGardeCyr Book',sans-serif;
    font-size: 14px;
    
  }
  input{
    border:1px solid #DDE2E4;
    border-radius: 8px;
    height: 40px;
    padding:11px 16px;
    margin-bottom: 30px;
    
  }
  button{
    width:153px;
    margin: 0 auto;
    background-color:#F0F5FF;
    padding:12px 40px;
    color:#314B99;
    font-size: 18px;
    border-radius: 8px;
   
  }
}

.overlay{
  position: fixed;
  top:0;
  left:0;
  width:100%;
  height: 100%;
  backdrop-filter: blur(2px);
  z-index: 10;
}

.input-add{
  border: 1px solid red;
}

.border{
  position: relative;
  color:#16191D
}
.border::before{
   content:'';
   height: 32px;
   width: 1px;
   position: absolute;
   left:0px;
   top:4px;
   background-color: rgb(196, 196, 196)
   
   
}

.table .elem{
  padding:20px 20px;

}


.elem input[type="checkbox"]{
  display:none
}

.elem input[type="checkbox"] + label{
  display:inline-block;
  width:20px;
  height: 20px;
  border: 1px solid #16191D;
  border-radius: 50%;
  cursor: pointer;
}

.elem input[type="checkbox"]:checked + label{
  background: url('../../public/img/checked.svg') 50% no-repeat;
  border:1px solid #134EC1;
}

.table td{
  border-top:1px solid #EEEBE9;
 
}

.header-second__sort select{
  appearance:none;
  -webkit-appearance:none;
  -moz-appearance:none;
  background: url('../../public/img/arrow-sort.svg') 100% no-repeat;
  background-size: 14px 12px;
  padding-right:5px;
}
</style>