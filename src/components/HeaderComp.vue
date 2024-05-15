<template>
    <header class="header">
      <div class="header-first">
          <h1 class="todo__title">To do list</h1>
          <button class="todo__add" @click="$emit('show-modal')">
            <img src="img/button-add.svg" alt="btn">
          </button>
      </div>
      <div class="header-second">
        <div class="header-second__loop">
            <img src="img/loop.svg" alt="">
        <input type="text" placeholder="Поиск Имени,статуса или даты" :value="searchTasks"
        @input="$emit('update:searchTasks',$event.target.value)"
        >
        </div>
        <div class="header-second__sort">
          Сортировать по:
          <select name="sort" class="header-second__sort" @change="$emit('update:sortBy', $event.target.value)">
            <option value="date">Дата</option>
            <option value="status">Статус</option>
          </select>
        </div>
      </div>
    </header>
</template>
<script>
export default {
    props:{
        searchTasks:{
            type:String,
            default:''
        },
        sortBy:{
            type:String,
            default:'date'
        }
    },
    methods:{
        searchTasksUpdate(value){
            this.$emit('update:searchTasks',value)
        },
        sortByUpdate(value){
            this.$emit('update:sortBy',value)
        }
        
       
    }
    
}
</script>
<style lang="scss">
    .header{
  margin-top:100px;
  margin-left: 30px;
  
    
  &-first{
    display:flex;
    justify-content: space-between;
  }
  &-second{
    margin-top:30px;
    display:flex;
    justify-content: space-between;
    @media(max-width:550px){
      flex-direction: column;

    }

    &__sort{
      @media(max-width:550px){
      margin-top:10px;
    }
    }
    input{
      flex:1 0 auto;
      width:100%;
      
      
    &::placeholder{
      color:#C4C4C4
    }

      
    }
    &__loop{
     display:flex;
      width:50%;
      img{
        margin-right:16px;
      }
    }
  }
}
</style>