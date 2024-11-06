<script setup>
import { ref, computed } from 'vue';

const inputText = ref('');
const Items = ref([]);
const checkedStates = ref([]);
const itemLength = computed(() => Items.value.length);
const completedCount = computed(() => checkedStates.value.filter(Boolean).length);

const addItem = () => {
  Items.value.push(inputText.value);
  checkedStates.value.push(false); 
  inputText.value = '';
};

const finishedTasks = ref([]);

const finishedItem = (index) => {
  if (checkedStates.value[index]) {
    finishedTasks.value.push(Items.value[index]);
  }
};

const edit = (id) => {
  inputText.value = Items.value[id];
  Items.value.splice(id, 1);
};

const deleteItem = (id) => {
  Items.value.splice(id, 1);
  checkedStates.value.splice(id, 1);
};

</script>

<template>
  <div class="outer-container">
    <div class="upper-rank">
      <h2>To-Do List</h2>
      <p id="list-title">What needs to be done?</p>
      <input type="text" id="input-text" v-model="inputText">
      <button @click="addItem" id="add-button">Add</button>
    </div>
    
    <div class="notes-list">
      <h2>{{completedCount}} out of {{itemLength}} items completed</h2>
      
      <div class="title" v-for="(item, index) in Items" :key="index">
        <div class="checkbox-wrapper">
          <input type="checkbox" v-model="checkedStates[index]" @change="finishedItem(index)">
        </div>
        <p>{{ item }}</p>
        <b-button squared variant="primary" id="edit-button" @click="edit(index)">Edit</b-button>
        <b-button squared variant="danger" @click="deleteItem(index)">Delete</b-button>
      </div>
    </div>
  </div>
</template>

<style>
.outer-container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center
}
.upper-rank{
  text-align: center;
  margin-bottom: 20px;
}
#list-title{
  font-size: 24px;
  font-weight: 700px;
  margin-bottom: 20px;  
}
.upper-rank > h2 {
  font-weight: bold;
}
#add-button{
  display: block;
  margin: auto;
  width: 35rem;
  background-color: black;
  color:white;
  border: 1px solid black;
  border-radius: 10px;
  padding: 8px;
}
#input-text{
  width: 35rem;
  margin-bottom: 15px;
}
.notes-list , h2{
  font-weight: 500px
}
.title{
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  align-items: center;
  gap: 7px;
}
.title > p{
  font-size: 18px;
  font-weight: 500;
  text-align: center;
}
.checkbox-wrapper {
  display: inline-block;
  border: 1px solid black;  
  width: 30px; 
  height: 30px; 
  position: relative; 
}

#checkbox {
  width: 100%; 
  height: 100%;
  margin: 0; 
  cursor: pointer; 
}
.buttons > button {
  width: 15rem;
}
#edit-button{
  margin-right: 10px;
}
</style>
