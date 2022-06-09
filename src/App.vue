<template>
  <div id=app>
    

    <div class="wrapper">
      <h2>{{ title }}</h2>
      <p>{{ upComing }}</p>
      <ul class="plates" v-if="items.length === 0">
        <li>{{ add }}</li>
      </ul>
      <ul class="plates" v-else>
        <li v-for="(item,index) in items" :key="index + 'items'">
            <input type="checkbox" :data-index="'data-index'+index" :id="'items'+index"
              :checked="item.done" />
            <label v-on:click="toggleDone(index)"
              :for="'item'+index">{{item.message}}</label>
            <button v-on:click='deleteItem(index)'>X</button>
        </li>
      </ul>
      <p>{{ finish }}</p>
      <ul class="plates">
        <li>{{ add }}</li>
      </ul>
      <ul class="plates">
        <li v-for="(item,index) in finishItems" :key="index + 'finishItems'">
           <input type="checkbox" :data-index="'data-index'+index" :id="'items'+index"
              :checked="item.done" />
            <label v-on:click="finishHandler()"
              :for="'item'+index">{{item.message}}</label>
            <button v-on:click='deleteFinish(index)'>X</button>
        </li>
      </ul>
      <form class="add-items" v-on:submit= "addItem($event)">
        <input type="text" name="item" placeholder="Item Name" required>
        <input type="submit" value="+項目">
      </form>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'App',
    data(){
      return{
        title:'TO DO list',
        upComing:'代辦項目',
        finish:'完成項目',
        add:'add...',
        items:[],
        finishItems:[],
      }
    },
    created(){
      const items = JSON.parse(localStorage.getItem('items')) || []
      this.items = items
    },
    methods:{
      deleteFinish: function(index){
          const deleteFinishitem = this.finishItems.filter((item,currentIndex)=>{
            return index != currentIndex
          })
          this.finishItems = deleteFinishitem
      },
      deleteItem: function(index){
          const newItems = this.items.filter((item,currentIndex)=>{
            return index != currentIndex
          })
          this.items = newItems;
          localStorage.setItem('items', JSON.stringify(newItems));
      },
      addItem: function(e) {
        e.preventDefault();
        let from = document.querySelector('.add-items')
        const text = (from.querySelector('[name=item]')).value;
        const item = {
          message: text,
          done: false 
        };
        this.items.push(item); 
        localStorage.setItem('items', JSON.stringify(this.items));
        from.reset();
      },
      toggleDone :function (index){
        this.items[index].done = !this.items[index].done;
        
        const handlerItems = this.items.filter((items,currentIndex)=>{
          return index != currentIndex
        })
        const checkedItems = this.items.filter((items,currentIndex)=>{
          return currentIndex === index
        })
        this.finishItems.push(checkedItems[0])
        console.log(this.finishItems)
        this.items = handlerItems
        //localStorage.setItem('items', JSON.stringify(this.items));
      },
      
    }
  }
  
</script>
<style>
html {
    box-sizing: border-box;
    background-size: cover;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    font-family: Futura, "Trebuchet MS", Arial, sans-serif;
  }
  
  *,
  *:before,
  *:after {
    box-sizing: inherit;
  }
  
  
  
  .wrapper {
    padding: 20px;
    max-width: 350px;
    background: rgba(255, 255, 255, 0.95);
    box-shadow: 0 0 0 10px rgba(0, 0, 0, 0.1);
  }
  
  h2 {
    text-align: center;
    margin: 0;
    font-weight: 200;
  }
  .plates {
    margin: 0;
    padding: 0;
    text-align: left;
    list-style: none;
  }
  .plates li {
    border-bottom: 1px solid rgba(0, 0, 0, 0.2);
    padding: 10px 0;
    font-weight: 100;
    display: flex;
  }
  
  .plates label {
    flex: 1;
    cursor: pointer;
  }
  
  .plates input {
    display: none;
  }
  
  .plates input + label:before {
    content: "⬜️ ";
    margin-right: 10px;
  }
  
  
  .plates input:checked + label:before {
    content: "V ";
  }
  
  .add-items {
    margin-top: 20px;
  }
  
  .add-items input {
    padding: 10px;
    outline: 0;
    border: 1px solid rgba(0, 0, 0, 0.1);
  }
</style>
