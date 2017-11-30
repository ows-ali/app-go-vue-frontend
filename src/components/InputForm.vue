<template>
  <div class="input">
    <p><input type="text" v-bind:class="isError?'error':''" v-bind:placeholder="tip" v-model="newItem" v-on:keyup.enter="addNewItem"/><button>add</button></p>
  </div>
</template>

<style>
  input{
    height: 30px;
    width: 200px;
    padding-left: 10px;
    border: 1px solid #aaa;  
  }

  .error{
    background: #fcc;
  }

  button{
    height: 34px;
    border: 1px solid #aaa;
    border-left: 0px;
    cursor: pointer;
    border-radius: 0 6px 6px 0;    
  }

</style>

<script>
export default {
  name: 'InputForm',
  data () {
    return {
      newItem: '',
      tip: 'please input something to do',
      isError: ''
    }
  },
  methods: {
    addNewItem () {
      if (this.newItem) {
        this.isError = false
        this.tip = 'please add another thing'
        this.$emit('addNewItem', this.newItem)
        this.newItem = ''
      } else {
        this.tip = 'can\'t add empty'
        this.isError = true
      }
    }
  }
}
</script>


