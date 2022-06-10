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
            <label v-on:click="reductionHandler(index)"
              :for="'item'+index">{{item.message}}</label>
            <button v-on:click='deleteFinish(index)'>X</button>
        </li>
      </ul>
      <form class="add-items" v-on:submit= "addItem($event)">
        <input type="text" name="item" placeholder="Item Name" required
          v-model="inputValue" @change="ghj()"
        >
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
        inputValue: '',
      }
    },
    created(){
      const items = JSON.parse(localStorage.getItem('items')) || []
      const finishItems = JSON.parse(localStorage.getItem('finishItems')) || []
      this.items = items
      this.finishItems = finishItems
    },
    methods:{
      reductionHandler: function(index){
        /**勾選時刪除完成項目功能 */
         const handlerItems = this.finishItems.filter((items,currentIndex)=>{
          return index != currentIndex
        })
        /**點選時完成項目跑回代辦項目 */
        const reductionItem = this.finishItems.filter((items,currentIndex)=>{  
          return index === currentIndex
        })
        let reductionClick = reductionItem[0]
        reductionClick.done = !reductionClick.done
        this.items.push(reductionClick)
        this.finishItems = handlerItems
         localStorage.setItem('items', JSON.stringify(this.items)); 
        localStorage.setItem('finishItems', JSON.stringify(this.finishItems));
      },
      /**  刪除完成項目的功能*/
      deleteFinish: function(index){ 
          const deleteFinishitem = this.finishItems.filter((item,currentIndex)=>{
            return index != currentIndex
          })
          this.finishItems = deleteFinishitem
          localStorage.setItem('items', JSON.stringify(deleteFinishitem));
      },
      /** 刪除代辦事項的功能 */
      deleteItem: function(index){ 
          const newItems = this.items.filter((item,currentIndex)=>{
            return index != currentIndex
          })
          this.items = newItems;
          localStorage.setItem('items', JSON.stringify(newItems));
      },
      /** 增加代辦事項功能 */
      addItem: function(e) {
        e.preventDefault();
        // let from = document.querySelector('.add-items')
        // const text = (from.querySelector('[name=item]')).value;
        const text = this.inputValue;
        const item = {
          message: text,
          done: false 
        };
        this.items.push(item); 
        localStorage.setItem('items', JSON.stringify(this.items));
        // from.reset();
        this.inputValue = ''
      },
      toggleDone :function (index){
        /** 勾選時代辦事項要消失 */
        const handlerItems = this.items.filter((items,currentIndex)=>{
          return index != currentIndex
        })
        /**勾選時代辦事項跑到完成項目 */
        const checkedItems = this.items.filter((items,currentIndex)=>{  
          return index === currentIndex
        })
        let clickedItem = checkedItems[0]
        clickedItem.done = !clickedItem.done
        this.finishItems.push(clickedItem)
        this.items = handlerItems
        localStorage.setItem('items', JSON.stringify(this.items)); 
        localStorage.setItem('finishItems', JSON.stringify(this.finishItems));
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
