<template>
  <div class="pagination">
    <ul>
      <li v-on:click="moveFirst"><a>首</a></li>
      <li v-bind:class="currentPage<=1?'disabled':''" v-on:click="movePrevious"><a>&lt;</a></li>
      <li v-for="pager in pagers" v-bind:key="pager" v-bind:class="currentPage==pager?'active':''" v-on:click="setCurrentPage(pager)"><a>{{pager}}</a></li>
      <li v-bind:class="currentPage>=totalPages?'disabled':''" v-on:click="moveNext"><a>&gt;</a></li>
      <li v-on:click="moveLast"><a>末</a></li>
      <li class="special"><span>每页 <input type="number" v-model="countPerPage" v-on:change="changeCountPerPage"/> 条</span></li>
      <li class="special"><span>共 {{totalCount}} 条 {{totalPages}} 页</span></li>
    </ul>      
  </div>  
</template>

<style scoped>

div.pagination{
  position: absolute;
  bottom: 20px;
  width: 60%;
  padding: 10px 20%;
  font-size: 10px;
}

ul{
  margin: 0 auto;
}

li.special{
  border: none;
  cursor: auto;
  padding: 0 2px;
}
  
  
  li{
    height: 20px;
    line-height: 20px;
    display: inline-block;
    list-style: none;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 0 10px;
    margin-left: 2px;
    margin-right: 0;
    cursor: pointer;
  }

  li.active{
    color: #00f;
    background: #fff;
  }  

  li.disabled>a{
    cursor: not-allowed;
  }

  input{
    width: 30px;
    height: 15px;
    border: 1px solid #ccc;
    background-color: transparent;
  }
</style>

<script>
export default {
  name: 'MyPagination',
  data () {
    return {
      countPerPage: '5',
      currentPage: '1'
    }
  },
  props: ['totalCount'],
  computed: {
    totalPages: function () {
      this.countPerPage = this.countPerPage > 5 ? this.countPerPage : 5
      return Math.ceil(this.$props.totalCount / this.countPerPage)
    },
    pagers: function () {
      let arr = []
      for (var i = 0; i < this.totalPages; i++) {
        arr[i] = i + 1
      }
      return arr
    }
  },
  methods: {
    setCurrentPage (pager) {
      this.currentPage = pager
      this.$emit('currentPageChanged', this.currentPage, this.countPerPage)
    },
    movePrevious () {
      if (this.currentPage > 1) {
        this.currentPage --
        this.$emit('currentPageChanged', this.currentPage, this.countPerPage)
      }
    },
    moveNext () {
      if (this.currentPage < this.totalPages) {
        this.currentPage ++
        this.$emit('currentPageChanged', this.currentPage, this.countPerPage)
      }
    },
    moveFirst () {
      this.currentPage = 1
      this.$emit('currentPageChanged', this.currentPage, this.countPerPage)
    },
    moveLast () {
      this.currentPage = this.totalPages
      this.$emit('currentPageChanged', this.currentPage, this.countPerPage)
    },
    changeCountPerPage () {
      if (this.currentPage > this.totalPages) {
        this.currentPage = this.totalPages
      }
      this.$emit('countPerPageChanged', this.countPerPage)
    }
  }
}
</script>


