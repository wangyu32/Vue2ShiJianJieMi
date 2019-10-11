<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <!-- <h1>{{ data2.title }}</h1> -->
    <h2>Essential Links</h2>
    <ul>
      <li v-for="v in this.p1">
        {{ v }}
      </li>
    </ul>

    <h2>待办事项11</h2>

    <ul class="todos">

      <li>
        <input v-model="newTodo" @keyup.13="addItem" autofocus="true" placeholder="写点东西"/>
      </li>

      <li v-for="(todo, index) in this.todos" :id="index" :class="{'checked':todo.done}">
        <input type="checkbox" @change="saveToStroe" v-model="todo.done" />
        <label> {{index + 1}}. {{ todo.value }} </label>
        <time> {{ todo.created | date}} </time>
        <button @click.prevent="delItem(todo)" />
      </li>
    </ul>

  </div>
</template>

<script>
import './assets/todos.less'
import moment from 'moment'
import 'moment/locale/zh-cn'
moment.locale('zh-cn')
export default {
  name: 'app',
  data () {
    return {
      newTodo : '',
      msg: 'Welcome to Your Vue.js App',
      title: 'vue-todos',
      p1 : {
        f_name : 'wang',
        l_name : 'yu'
      },
      todos:[
         {value : "阅读一本书", done : false},
         {value : "补充代码", done : true},
         {value : "写心得", done : false}
      ]
    }
  },
  created(){
    if(this.is_initialized){
      this.todos = JSON.parse(localStorage.getItem('VUE-TODOS'))
    }
  },

  computed: {
    is_initialized(){
      return localStorage.getItem('VUE-TODOS') != null
    }
  },

  filters: {
    date(val){
      // return val
      return moment(val).calendar()
    }
  }, 
  methods: {
      addItem(){
        this.todos.push({
          value:this.newTodo,
          created: Date.now(),
          done:false
        });
        this.saveToStore();
        this.newTodo = ''
      },
      delItem(todo){
        this.todos = this.todos.filter((x) => x !== todo)
        this.saveToStore()
      },
      saveToStore(){
        localStorage.setItem('VUE-TODOS', JSON.stringify(this.todos))
      }
      
  }



  // , 写data2不被支持
  // data2 () {
  //   return {
  //      title: 'test-vue'
  //   }
  // }
}
</script>

