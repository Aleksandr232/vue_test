<template>
  <div class="col-md-12">
    <div class="form-group">
      <input type="text" class="form-control" v-model="search" placeholder="Search">
    </div>
    <div class="table-responsive">
      <table class="table table-striped table-bordered" style="width:100%">
          <thead width="400px">
              <tr>
                  <th scope="col" @click="sort('id')">id<i class="fas fa-sort-alpha-down float-right"></i></th>
                  <th scope="col" @click="sort('firstName')"> firstName <i class="fas fa-sort-alpha-down float-right"></i></th>
                  <th scope="col" @click="sort('lastName')">lastName <i class="fas fa-sort-alpha-down float-right"></i></th>
                  <th scope="col" @click="sort('email')">email<i class="fas fa-sort-alpha-down float-right"></i></th>
                  <th scope="col" @click="sort('phone')">phone<i class="fas fa-sort-alpha-down float-right"></i></th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="(rows, index) in (sortedActivity, filteredList)" :key="index">
                <td>{{rows.id}}</td>
                <td>{{rows.firstName}}</td>
                <td>{{rows.lastName}}</td>
                <td>{{rows.email}}</td>
                <td>{{rows.phone}}</td>
              </tr>
          </tbody>
      </table>
    </div>
  </div>
</template>

<script>
/*eslint-disable*/
import axios from 'axios';

export default {
  data: () => ({
    rows: [],
    currentSort:'name',
    currentSortDir:'asc',
    search: '',
    searchSelection: '',
    pageSize: 5,
    currentPage: 1
  }),

  methods:{
    sort:function(s) {
      if(s === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
      }
      this.currentSort = s;
    },
    nextPage:function() {
      if((this.currentPage*this.pageSize) < this.users.length) this.currentPage++;
    },
    prevPage:function() {
      if(this.currentPage > 1) this.currentPage--;
    }
  },

  computed: {
    sortedActivity:function() {
      return this.rows.sort((a,b) => {
        let modifier = 1;
        if(this.currentSortDir === 'desc') modifier = -1;
        if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;
      }).filter((row, index) => {
        let start = (this.currentPage-1)*this.pageSize;
        let end = this.currentPage*this.pageSize;
        if(index >= start && index < end) return true;
      });
    },

    filteredList () {
      return this.rows.filter((data) => {
        let id = data.id.toLowerCase().match(this.search.toLowerCase());
        let firstName = data.firstName.toLowerCase().match(this.search.toLowerCase());
        let lastName = data.lastName.toLowerCase().match(this.search.toLowerCase());
        let email = data.email.toLowerCase().match(this.search.toLowerCase());
        let phone = data.phone.toLowerCase().match(this.search.toLowerCase());
        return id || firstName || lastName || phone  || email ;
      }).filter((row, index) => {
        let start = (this.currentPage-1)*this.pageSize;
        let end = this.currentPage*this.pageSize;
        if(index >= start && index < end) return true;
      });
    }
  },

  created () {
    axios.get(`http://www.filltext.com/?rows=1000&id=${number|1000}&firstName=${firstName}&delay=3&lastName=${lastName}&email=${email}&phone={phone|(xxx)xxx-xx-xx}}`)
        .then(response => {
        this.users = response.data
      })
  },

}
</script>

<style>
th {
  cursor:pointer;
  /* width: 500px !important; */
  white-space: nowrap;
}

tr {
  white-space: nowrap;
}

</style>
