<template>
  <div v-if="totalCount" class="fe-page"> 
    <div class="fe-page-linkBox">
      <span class="fe-page-prev fe-btn" 
        :class="{'disabled': !canPre}"
        @click="setCurrentPage(selfCurrentPage-1)">
          <
      </span>
      <span v-for="n in frontArr"
        :class="['fe-page-item', { 'fe-btn' : true , 'active': selfCurrentPage == n }]"
        @click="setCurrentPage(n)"
        :key="n">{{n}}</span>        
      </span>
      <span class="fe-page-omit" v-if="showFrontOmit">...</span>     
      <span
        v-for="n in midArr"
        :class="['fe-page-item', { 'fe-btn' : true , 'active': selfCurrentPage == n }]"
        @click="setCurrentPage(n)"
        :key="n">{{n}}
      </span>     
      <span class="fe-page-omit" v-if="showEndOmit">...</span>
      <span v-for="n in endArr"
         :class="['fe-page-item', { 'fe-btn' : true , 'active': selfCurrentPage == n }]"
         @click="setCurrentPage(n)"
         :key="n">{{n}}</span>       
      <span class="fe-page-prev fe-page-btn" 
        :class="{'disabled': !canNext}"
        @click="setCurrentPage(selfCurrentPage+1)">
          >
      </span>
    </div>
    <div class="fe-page-operateBox">
      <span class="fe-page-span">到</span>
      <input v-model="gotoVal" @keyup.enter="setCurrentPage(gotoVal)" class="fe-page-input">
      <span class="fe-page-span">页</span>
      <button class="fe-page-btn"
        @click="setCurrentPage(gotoVal)">
          确定
      </button>
    </div>
  </div>
</template>
<script>
  export default{
    props: {
      currentPage: {
        default: 1,
        type: Number
      },
      totalCount: {
        default: 0,
        type: Number
      },
      pageSize: {
        default: 10,
        type: Number
      },
      pageHolderLength: {
        default: 9,
        type: Number
      }
    },
    data () {
      return {
        selfCurrentPage: 1,
        gotoVal: 1
      }
    },
    watch: {
      currentPage (val) {
        this.selfCurrentPage = val
      }
    },
    computed: {
      totalPage () {
        return Math.ceil(this.totalCount / this.pageSize)
      },
      canPre () {
        return this.selfCurrentPage != 1
      },
      canNext () {
        return this.selfCurrentPage != this.totalPage
      },
      offset() {
        return Math.floor(this.pageHolderLength / 2)
      },
      showFrontOmit () {
        return this.totalPage > this.pageHolderLength && this.selfCurrentPage > this.offset
      },
      showEndOmit () {
        return this.totalPage > this.pageHolderLength && this.selfCurrentPage < this.totalPage - this.offset
      },
      frontArr () {
        if (this.showFrontOmit) {
          return [1]
        } else if (this.showEndOmit) {
          return this.getNumArray(1, 7)
        } else {
          return this.getNumArray(1, this.totalPage)
        }
      },
      endArr () {
        if (this.showEndOmit) {
          return [this.totalPage]
        } else if (this.showFrontOmit) {
          return this.getNumArray(this.totalPage - 6, this.totalPage)
        } else {
          return []
        }
      },
      midArr () {
        if (this.showFrontOmit && this.showEndOmit) {
          const deltaA = Math.floor(this.offset / 2)
          const deltaB = Math.ceil(this.offset / 2)
          return this.getNumArray(this.selfCurrentPage - deltaA, this.selfCurrentPage + deltaB)
        } else {
          return []
        }
      }
    },
    methods: {
      setCurrentPage (number) {
        if (this.__verifyChangeNumber(number)) {
          this.selfCurrentPage = Number(number)
          this.$emit('update:currentPage', this.selfCurrentPage)
        }
      },
      getNumArray(begin, end) {
        var array = [];
        for (var i = begin; i <= end; i++) {
          array.push(i)
        }
        return array;
      },
      __verifyChangeNumber(number) {
        return number >= 1 && number <= this.totalPage && this.selfCurrentPage != number
      }      
    }
  }
</script>
<style>
  .fe-page {
    display: block;
    color: #959595;
    font-size: 12px;
    text-align: center;
  }
  .fe-page:after {
    display: block;
    clear: both;
    width: 0px;
    height: 0px;
    content: " ";
  }
  .fe-page .fe-page-linkBox,
  .fe-page .fe-page-operateBox {
    display: inline-block;
  }
  .fe-page .fe-page-linkBox:after,
  .fe-page .fe-page-operateBox:after {
    display: block;
    clear: both;
    width: 0px;
    height: 0px;
    content: " ";
  }
  .fe-page .fe-page-linkBox .fe-page-prev,
  .fe-page .fe-page-linkBox .fe-page-next,
  .fe-page .fe-page-linkBox .fe-page-item,
  .fe-page .fe-page-linkBox .fe-page-omit {
    float: left;
    margin-left: 8px;
    padding: 0px 6px;
    height: 30px;
    line-height: 32px;
    min-width: 18px;
    border: 1px solid #F5F5F5;
    background: #F5F5F5;
    text-align: center;
    font-size: 12px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    -o-user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    -o-user-select: none;
    user-select: none;
  }
  .fe-page .fe-page-linkBox .fe-page-item:hover,
  .fe-page .fe-page-linkBox .fe-page-item.active {
    border: 1px solid #316ccb;
    background: #fff;
    color: #316ccb;
    cursor: pointer;
  }
  .fe-page .fe-page-linkBox .fe-page-prev:hover,
  .fe-page .fe-page-linkBox .fe-page-next:hover {
    cursor: pointer;
  }
  .fe-page .fe-page-linkBox .fe-page-prev.disabled,
  .fe-page .fe-page-linkBox .fe-page-next.disabled,
  .fe-page .fe-page-linkBox .fe-page-prev.disabled:hover,
  .fe-page .fe-page-linkBox .fe-page-next.disabled:hover {
    border-color: #fff;
    color: #e5e5e5;
    cursor: default;
  }
  .fe-page .fe-page-operateBox {
    margin-left: 40px;
  }
  .fe-page .fe-page-operateBox .fe-page-input,
  .fe-page .fe-page-operateBox .fe-page-span,
  .fe-page .fe-page-operateBox .fe-page-btn {
    float: left;
    height: 32px;
    line-height: 32px;
  }
  .fe-page .fe-page-operateBox .fe-page-input {
    margin-left: 8px;
    margin-right: 8px;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;
    -o-box-sizing: border-box;
    box-sizing: border-box;
    width: 64px;
    border: 1px solid #F5F5F5;
    background: #fff;
    text-align: center;
    vertical-align: middle;
    outline: none;
  }
  .fe-page .fe-page-operateBox .fe-page-input:focus {
    border-color: #316ccb;
    padding: 0 3px;
  }
  .fe-page .fe-page-operateBox .fe-page-btn {
    margin-left: 24px;
    padding: 0px;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;
    -o-box-sizing: border-box;
    box-sizing: border-box;
    width: 64px;
    border: 1px solid #F5F5F5;
    background: #fff;
    color: #656565;
    cursor: pointer;
  }
  .fe-page .fe-page-operateBox .fe-page-btn:hover {
    border-color: #316ccb;
    background: #316ccb;
    color: #fff;
  }
  .fe-page-gray {
    display: inline-block;
    color: #959595;
    font-size: 12px;
  }
  .fe-page-gray:after {
    display: block;
    clear: both;
    width: 0px;
    height: 0px;
    content: " ";
  }
  .fe-page-gray .fe-page-linkBox,
  .fe-page-gray .fe-page-operateBox {
    float: left;
  }
  .fe-page-gray .fe-page-linkBox:after,
  .fe-page-gray .fe-page-operateBox:after {
    display: block;
    clear: both;
    width: 0px;
    height: 0px;
    content: " ";
  }
  .fe-page-gray .fe-page-linkBox .fe-page-prev,
  .fe-page-gray .fe-page-linkBox .fe-page-next,
  .fe-page-gray .fe-page-linkBox .fe-page-item,
  .fe-page-gray .fe-page-linkBox .fe-page-omit {
    float: left;
    margin-left: 8px;
    padding: 0px 6px;
    height: 30px;
    line-height: 32px;
    min-width: 18px;
    border: 1px solid #f5f5f5;
    background: #f5f5f5;
    text-align: center;
    font-size: 12px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    -o-user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    -o-user-select: none;
    user-select: none;
  }
  .fe-page-gray .fe-page-linkBox .fe-page-item:hover,
  .fe-page-gray .fe-page-linkBox .fe-page-item.active {
    border: 1px solid #316ccb;
    background: #fff;
    color: #316ccb;
    cursor: pointer;
  }
  .fe-page-gray .fe-page-linkBox .fe-page-prev:hover,
  .fe-page-gray .fe-page-linkBox .fe-page-next:hover {
    cursor: pointer;
  }
  .fe-page-gray .fe-page-linkBox .fe-page-prev.disabled,
  .fe-page-gray .fe-page-linkBox .fe-page-next.disabled,
  .fe-page-gray .fe-page-linkBox .fe-page-prev.disabled:hover,
  .fe-page-gray .fe-page-linkBox .fe-page-next.disabled:hover {
    border-color: #f5f5f5;
    background: #f5f5f5;
    color: #e5e5e5;
    cursor: default;
  }
  .fe-page-gray .fe-page-operateBox {
    margin-left: 40px;
  }
  .fe-page-gray .fe-page-operateBox .fe-page-input,
  .fe-page-gray .fe-page-operateBox .fe-page-span,
  .fe-page-gray .fe-page-operateBox .fe-page-btn {
    float: left;
    height: 32px;
    line-height: 32px;
  }
  .fe-page-gray .fe-page-operateBox .fe-page-input {
    margin-left: 8px;
    margin-right: 8px;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;
    -o-box-sizing: border-box;
    box-sizing: border-box;
    width: 64px;
    border: 1px solid #e5e5e5;
    background: #fff;
    text-align: center;
    vertical-align: middle;
    outline: none;
  }
  .fe-page-gray .fe-page-operateBox .fe-page-input:focus {
    border-color: #316ccb;
    padding: 0 3px;
  }
  .fe-page-gray .fe-page-operateBox .fe-page-btn {
    margin-left: 24px;
    padding: 0px;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;
    -o-box-sizing: border-box;
    box-sizing: border-box;
    width: 64px;
    border: 1px solid #e5e5e5;
    background: #fff;
    color: #656565;
    cursor: pointer;
  }
  .fe-page-gray .fe-page-operateBox .fe-page-btn:hover {
    border-color: #316ccb;
    background: #316ccb;
    color: #fff;
  }
  /* 临时覆盖 后面修改 fe-page */
  .fe-page .fe-page-linkBox .fe-page-prev,
  .fe-page .fe-page-linkBox .fe-page-next,
  .fe-page .fe-page-linkBox .fe-page-item,
  .fe-page .fe-page-linkBox .fe-page-omit {
    margin-left: 0px;
    margin-right: 8px;
  }  
</style>