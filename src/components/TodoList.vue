<template>
  <div class="todo">
    <input-form v-on:addNewItem="addNewItem"></input-form>
    <ul>
      <list-item v-for="(item,index) in items" v-bind:item="item" v-bind:key="index" v-bind:index="index" v-on:remove="remove(index)" v-show="isShow(index)"></list-item>
    </ul>
    <my-pagination v-bind:total-count="items.length" v-on:currentPageChanged="currentPageChanged" v-on:countPerPageChanged="countPerPageChanged"></my-pagination>
    <div class="introduction">
      <Introduction></Introduction>
    </div>    
  </div>
</template>

<style>

  .todo{
    width: 50%;
    min-height: 400px;
    height: auto;
    border: 1px solid #ccc;
    border-radius: 10px;
    margin: 0 auto;
    padding: 10px;
    padding-bottom: 60px;
    background: #ffc;
    position: relative;
  }

  ul{
    margin-left: 25%; 
    margin-right: 25%;   
  }

  .introduction{
    position: absolute;
    width: 220px;
    right: 10px;
    top: 180px;
  }

  

</style>

<script>

import ListItem from './ListItem.vue'
import InputForm from './InputForm.vue'
import MyPagination from './MyPagination.vue'
import Introduction from './Introduction.vue'

export default {
  name: 'TodoList',
  components: {
    InputForm, ListItem, MyPagination, Introduction
  },
  data () {
    return {
      newItem: '',
      items: [
        {
          status: 'finished',
          content: 'learn vue 2.0'
        },
        {
          status: 'pending',
          content: 'learn Java'
        },
        {
          status: 'finished',
          content: 'learn React'
        },
        {
          status: 'pending',
          content: 'learn single vue component'
        },
        {
          status: 'finished',
          content: 'learn vue Router'
        },
        {
          status: 'pending',
          content: 'learn vuex'
        },
        {
          status: 'finished',
          content: 'learn webpack'
        },
        {
          status: 'pending',
          content: 'learn git and npm'
        }
      ],
      itemsPerPage: 5,
      currentPage: 1
    }
  },
  computed: {
    totalPages: function () {
      return Math.ceil(this.items.length / this.itemsPerPage)
    }
  },
  methods: {
    adjustPage () {
      if (this.currentPage > this.totalPages) {
        this.currentPage = this.totalPages
      }
    },
    addNewItem (newItem) {
      this.items.unshift({status: 'pending', content: newItem})
    },
    remove (index) {
      this.items.splice(index, 1)
      this.adjustPage()
    },
    currentPageChanged (currentPage, itemsPerPage) {
      this.itemsPerPage = itemsPerPage
      this.currentPage = currentPage
    },
    countPerPageChanged (countPerPage) {
      this.itemsPerPage = countPerPage
      this.adjustPage()
    },
    isShow (index) {
      return (index >= (this.currentPage - 1) * this.itemsPerPage) && (index < this.currentPage * this.itemsPerPage)
    }
  }
}
</script>


