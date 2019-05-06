<template>
  <div class="mod-app">
    <ul>
      <li v-for="item in dataList">
        {{item.text}}
      </li> 
    </ul>    
    <pager 
      :currentPage.sync="currentPage"
      @update:currentPage="getList(currentPage)"
      :totalCount="totalCount" 
      :pageSize="pageSize">
    </pager> 
  </div>
</template>

<script>  
  // import componentTest from '../src/index.vue'
  import pager from '../src/pager.vue'
  // import pager from '../src/index.js'
  // import pager from '../dist/index.min.js'
  export default {
    name: 'app',
    components: {
      pager
    },
    data () {
      return {
        totalCount: 100,
        pageSize: 3,
        dataList : [],
        currentPage: 1
      }
    },
    methods: {
      getList(num) {
        var list = [
          {'text': '哈哈哈'},
          {'text': '呵呵呵'},
          {'text': '哈哈哈'} 
        ]
        this.dataList = list
      }
    },
    mounted () {
      this.getList(1)
    }
  }
</script>
