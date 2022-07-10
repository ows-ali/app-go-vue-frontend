<template>
  <div style="width:80%; margin-left: auto; margin-right: auto;">
    <h1 style="text-align: center">Order list</h1>
    <vue-good-table
      :columns='columns'
      :line-numbers="true"
      :rows='rows'
      mode="pages"

      @on-page-change="onPageChange"
      @on-sort-change="onSortChange"
      @on-column-filter="onColumnFilter"
      :isLoading.sync="isLoading"

      :pagination-options="{
        perPageDropdown: [5],
        dropdownAllowAll: false,
        perPageDropdownEnabled: false,
        setCurrentPage: 1,

        perPage: 5,

        enabled: true,
      }"
      :search-options="{
        enabled: true,
        trigger: 'enter',
        skipDiacritics: true,
        searchFn: mySearchFn,
        placeholder: 'Search this table',
        externalQuery: searchQuery,
      }"
      :totalRows="totalRecords"
      >
      
      </vue-good-table>
  </div>
</template>

<script>
export default {
  name: 'my-component',
  mounted () {
    this.loadItems()
  },
  methods: {
    updateParams (newProps) {
      this.serverParams = Object.assign({}, this.serverParams, newProps)
    },
    onPageChange (params) {
      this.updateParams({page: params.currentPage})
      this.loadItems()
    },

    onPerPageChange (params) {
      this.updateParams({perPage: params.currentPerPage})
      this.loadItems()
    },

    onSortChange (params) {
      console.log('paramsss', params)
      console.log('paramsss', params[0])
      // console.log('paramsss', params.columnIndex)

      // this.updateParams({
      //   sort: [{
      //     type: params.sortType,
      //     field: this.columns[params.columnIndex].field
      //   }]
      // })

      // this.updateParams({
      //   sort: [{
      //     type: params.sortType,
      //     field: this.columns[params.columnIndex].field
      //   }]
      // })
      this.updateParams({
        sort: [{
          type: params[0].type,
          field: params[0].field
        }]
      })
      // this.loadItems()
    },
    onColumnFilter (params) {
      this.updateParams(params)
      this.loadItems()
    },

    // load items is what brings back the rows from server
    loadItems (orderNameSearch = '') {
      console.log(' in load items', this.serverParams)
      console.log(' in load items', this.serverParams.page)
      console.log(' in load items', this.serverParams.page === 2)
      // getFromServer(this.serverParams).then(response => {
      //   this.totalRecords = response.totalRecords
      //   this.rows = response.rows
      // })
      let url = 'http://127.0.0.1:3000/orders'
      if (this.serverParams.page === 2) {
        url += '?offset=5'
      } else if (orderNameSearch !== '') {
        url = url + '?order_name=' + orderNameSearch
      }
      fetch(url)
      .then(data => {
        return data.json()
      })
      .then(data => {
        console.log('data', data)
        this.rows = data
      })
      .catch(err => {
        console.log('api err', err)
      })
    },
    formatFn: function (value) {
      return '$' + value
    }
  },
  data () {
    return {
      isLoading: false,
      totalRecords: 10,
      serverParams: {
        columnFilters: {
        },
        sort: [
          {
            field: '',
            type: ''
          }
        ],
        page: 1,
        perPage: 5
      },
      columns: [
        {
          label: 'Order Name',
          field: 'order_name',
          sortable: false
        },
        {
          label: 'Order Date',
          field: 'created_at',
          type: 'date',
          dateInputFormat: 'MMM dd, yyyy hh:mm a',
          dateOutputFormat: 'MMM do yy hh:mm a',
          globalSearchDisabled: true
        },

        {
          label: 'Customer Name',
          field: 'customer_name',
          sortable: false,
          globalSearchDisabled: true
        },

        {
          label: 'Customer Company Name',
          field: 'company_name',
          sortable: false,
          globalSearchDisabled: true
        },
        {
          label: 'Total Amount',
          field: 'total_amount',
          sortable: false,
          formatFn: this.formatFn,
          globalSearchDisabled: true
        },
        {
          label: 'Delivered Amount',
          field: 'delivered_amount',
          sortable: false,
          formatFn: this.formatFn,
          globalSearchDisabled: true
        }
      ],
      rows: [
      ]
    }
  }
}
</script>