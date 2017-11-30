<template>
  <li v-bind:class="item.status" v-on:dblclick="edit(item)">
    {{index+1}}.
    <span class="input" v-if="!editing"> {{item.content}}</span>
    <span v-else><input class="input editing" type="text" v-model="editedValue" v-on:blur="editing=false" v-on:change="item.content=editedValue"/></span>
    <button class="button" v-on:click="remove">&times;</button>
  </li>
</template>

<style>

  li{
    text-align: left;
    height: 30px;
    line-height: 30px;
    border-bottom: 1px solid #ccc;
    list-style: none;
  }

  .input{
    background: transparent;
    border: 0px;
    padding: 0;
    font-size: 16px;
    padding: 0px 6px;
  }

  .editing{
    background: #fff;
  }

  li.pending .input{
    color: #900;    
  }

  li.finished .input{
    color: #aaa;
  }

  .button{
    width: 20px;
    height: 20px;
    border: 0px solid #999;
    background: transparent;
    border-radius: 50%;    
    opacity: 0;
    cursor: pointer;
  }

  li:hover{
    background: #ffd;
  }

  li:hover .button{
    opacity: 1;
    float: right;
    margin-top: 5px;
    margin-right: 5px;
  }

</style>

<script>
export default {
  name: 'ListItem',
  props: ['item', 'index'],
  data () {
    return {
      editing: false,
      editedValue: ''
    }
  },
  methods: {
    remove () {
      this.$emit('remove')
    },
    edit (item) {
      this.editing = true
      this.editedValue = item.content
    }
  }
}
</script>


