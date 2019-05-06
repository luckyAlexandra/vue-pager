
vue-page
---------------------

基于vue2.x的分页组件

### Install
npm install vue-page

### Usage

```
<pager 
  :currentPage.sync="currentPage"
  @update:currentPage="handleUpdate(currentPage)"
  :totalCount="totalCount" 
  :pageSize="pageSize">
</pager> 
```
currentPage //当前页

totalCount //总条数

pageSize //每页条数

pageHolderLength //页码块占位数