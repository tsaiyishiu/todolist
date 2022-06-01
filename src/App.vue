<template>
  <div id=app>
    

    <div class="wrapper">
      <h2>{{ title }}</h2>
      <p>{{ Upcoming }}</p>
      <ul class="plates" v-on:click="toggleDone($event);inputclick($event)">
        <li>{{ add }} </li>
      </ul>
      <p>{{ Finish }}</p>
      <ul class="plates" v-on:click= "toggleDone($event)">
        <li>{{ add }}</li>
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
        Upcoming:'代辦項目',
        Finish:'完成項目',
        add:'add...',

    
      }
    },
    mounted(){
      console.log('123')
      this.newfunction()
      
    },
    methods:{
      
      newfunction: function(){
        console.log('newfunction')
        let itemsList = document.querySelector('.plates');
        const newclick = document.querySelectorAll('label');
        newclick.forEach((label,index)=>{
          label.addEventListener('click',()=>{
            const items = JSON.parse(localStorage.getItem('items')) || [];
            const newItems = items.filter((item,currentIndex)=>{
              return index != currentIndex   
            })
            this.populateList(newItems, itemsList);
            localStorage.setItem('items', JSON.stringify(newItems));
            this.newfunction()
          })
        })
      },
      addItem: function(e) {
        let itemsList = document.querySelector('.plates');
        const items = JSON.parse(localStorage.getItem('items')) || []; //原本的寫法可以先讀這段 vue的版本不行   
        e.preventDefault();
        console.log(e)
        let from = document.querySelector('.add-items')
          
        const text = (from.querySelector('[name=item]')).value;
          
        const item = {
          text: text,
          done: false 
        };
          
        items.push(item); 
        this.populateList(items, itemsList); 
        localStorage.setItem('items', JSON.stringify(items));
        console.log( JSON.stringify(items));
          
        from.reset();
        this.newfunction()
      },
      populateList: function (plates = [], platesList) {
        platesList.innerHTML = plates.map((plate, i) => {   
        
        return `
          <li>
            <input type="checkbox" data-index=${i} id="item${i}" ${plate.done ? 'checked' : ''} />
            <label for="item${i}">${plate.text}</label>
            <button>X</button>                                                  
          </li>
        `;
        }).join('');
          
      },
      toggleDone :function (e){
        console.log(e)
        let itemsList = document.querySelector('.plates');
        let items = JSON.parse(localStorage.getItem('items')) || [];
        if (!e.target.matches('input')) return;
        const el = e.target;
        const index = el.dataset.index;
        items[index].done = !items[index].done;
        localStorage.setItem('items', JSON.stringify(items));
        (items, itemsList);
      },
      inputclick :function(e){
          //let input = document.querySelector('input')
        console.log(e.target)
      },
        
        

      //先抓到勾選的click事件
      //
      
    },
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
