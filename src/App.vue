

<template>
  <div id="app">
    <div class="input-block"
      tabindex="0"
      v-on-click-outside="closeAllSelect"
      @keydown.tab ="next($event)"
      @keyup.enter="close($event)"
      @keyup.esc="close($event, 'close')"
    >
        <label class="label" id="label-1"   >
        <vue-numeric-input
          v-model="val"
          @focus="selectText($event)"
          controls-type="updown"
          :min="0"
          :step="100"
          ref="input1"
        />
         <a @click.stop="val = val1*2" >Сумма</a>
         <div class="label-start"
          @click.self="startClick($event, 'val'), val = 0"></div>
        </label>  
        <label class="label" id="label-2"  >
        <vue-numeric-input
          v-model="val1"
          @focus="selectText($event)"
          controls-type="updown"
          :min="0"
          :step="100"
          ref="input2"
        />
         <a  @click.stop="val1 = 1000" >Константа</a>
         <div class="label-start" @click.self="startClick($event, 'val1'), val1 = 0"></div>
        </label>  
        <label class="label" id="label-3" >
        <vue-numeric-input
          v-model="val1"
          @focus="selectText($event)"
          controls-type="updown"
          :min="0"
          :step="100"
          ref="input3"
        />
         <div class="label-start" @click.self="startClick($event, 'val1'), val1 = 0"></div>
        </label>  
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueNumericInput from 'vue-numeric-input'
import { mixin as onClickOutside } from 'vue-on-click-outside'
Vue.use(VueNumericInput)

export default {
  name: 'app',
  data () {
    return {
      param: null
    }
  },
  mixins: [onClickOutside],
  computed: {
   val2() {
      return this.val1
    },
    val:  {
      get:function(){
         return this.$store.getters.val
      },
      set: function (val) {
        this.$store.commit('VAL', val)
      }
    },
    val1:  {
      get:function(){
         return this.$store.getters.val1
      },
      set: function (val) {
        this.$store.commit('VAL1', val)
      }
    },
  },
  methods: {
    next(event) {
      const next = event.target.parentNode.parentNode.nextElementSibling
      const last = document.querySelector('#label-3')
      if (!this.checkFocus) return
      if (last.classList.contains("label-visible")){ 
       return
      } else {
        next.className = 'label-visible label '
        next.querySelector('.numeric-input').focus()
      }
      event.target.select()
      this.close(event)
      
    },
    checkFocus(){
     return this.$refs.input1 == document.activeElement ||  this.$refs.input2 == document.activeElement || this.$refs.input3 == document.activeElement
    },
    selectText(event) {
      event.target.select()
    },
    closeAllSelect(){
      let els = document.getElementsByClassName("label");
        if (event.target !== els){
          for (let label of els) {
          label.classList.remove("label-visible")
          }
      }
    },
    close(event, param){
      const child = event.target
      const parent = child.parentNode.parentNode

      if (param === 'close'){
        this.param === 'val' ? this.val = 0 : this.val1 = 0
      }
      parent.className = 'label'
    },
    startClick(event, param){
      const child = event.target
      const parent = child.parentNode
      this.closeAllSelect()
      this.param = param
      child.param = param
      parent.className = 'label label-visible'
    }
    
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: blue;
}
.mb-5{
  margin-bottom: 20px
}
 .input-block{
   height: 40px;
   width: 150px;
   text-align: left;
   margin: 10px auto;
 }
.vue-numeric-input .numeric-input{
  min-width: 150px; 
}
.label{
  height: 48px;
  margin-bottom: 20px;
  offset: none !important;
  outline: none !important;
}
.input-block{
  position: relative;
  display: flex;
  flex-direction: column;
  offset: none !important;
  outline: none !important;
}
.label{
  position: relative;
  .label-start{
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
  }
  &.label-visible{
    .label-start{
    display: none;
  }
  }
}
.label:not(.label-visible){
  display: inline-flex;
  align-items: center;
  
  .vue-numeric-input{
    max-width: 50px;
  }
  .btn, a{
    display: none;
  }
  input{
    border: 0;
    box-shadow: none;
    width: 30px;
    color: #000;
    box-shadow: none;
    outline: none;
    height: 30px;
  }
  
  &:after {
    content:'';
    display: block;
    border: solid black;
    border-width: 0 1px 1px 0;
    display: inline-block;
    padding: 2px;
    left: 50%;
    transform: translate(-50%, -50%)  rotate(45deg);
  }

  &:hover{
    input{
      color: blue;
    }
    .chevron{
      border-color: blue;
      
    }
  }
}
 .vue-numeric-input .numeric-input{
  height: 30px;
}
</style>
