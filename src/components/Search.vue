<template>
  <div class="wrap">
    <div class="search-box">
      <input
        type="text"
        class="searchInp"
        @blur="inputBlur"
        @focus="inputFocus"
        @input="inputContent"
        v-model="searchText"
        ref="input"
      />
      <div class="clear" v-show="focusState && searchText" @click="clear">
        <img src="../assets/img/X.svg" alt="" />
      </div>
      <input type="submit" value="百度一下" class="sub" />
    </div>
    <div class="list-box" v-show="showList" ref="listBox">
      <ul>
        <li v-for="item in filterData" :key="item.id" @click="setData(item)">
          {{ item.text }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, nextTick } from 'vue'
import data from '../assets/data/keyword.json'
let searchText = ref('')
const input = ref(null)
const listBox = ref(null)
const focusState = ref(false)
let showList = ref(false)


// 动态过滤数据
let filterData = computed(() => {
  return data.filter((item) => {
    return item.text.indexOf(searchText.value) == 0
  })
})
// 点击里查询列表项时，同步数据
function setData(item) {
  searchText = item.text
  showList.value = false
}
// 输入内容时
function inputContent() {
  if (!searchText.value) {
    showList.value = false
  } else {
    console.log(filterData);
    showList.value = true
  }
}
// input失焦时
function inputBlur() {
  focusState.value = false
}
// 清除按钮
function clear() {
  console.log(3333);
  searchText = ''
  
  showList.value = false
  focusState.value = false
  nextTick(()=>{
    console.log(233);
    input.value.value = ''
  })
}
// input框聚焦时
function inputFocus() {
   focusState.value = true
   showList.value =true
  
  if (!searchText.value) {
    // 搜素框里没内容
    showList.value = false
  } else {
    console.log(213)

    showList.value = true
  }
}
</script>

<style lang="scss" scoped>
.wrap {
  margin: 100px auto;
  width: 546px;
  position: relative;
  .list-box {
    position: absolute;
    top: 36px;
    width: 434px;
    // height: 400px;
    border: 2px solid #4e6ef2;
    background-color: #fff;
    border-top: none;
    border-radius: 0 0 10px 10px;
    overflow: hidden;
    ul {
      margin-top: 6px;
      // margin-left: 15px;
      width: 100%;
      border-top: 1px solid #e8e8e8;
      li {
        // width: 100%;
        height: 30px;
        line-height: 30px;
        padding-left: 20px;
        cursor: pointer;
        &:hover {
          background-color: #e8e8e8;
        }
      }
    }
  }
  .search-box {
    width: 546px;
    height: 44px;
    // line-height: 44px;
    box-sizing: border-box;
    border: 2px solid #4e6ef2;
    border-radius: 10px;
    overflow: hidden;
    position: relative;

    .searchInp {
      // border: 1px solid;
      width: 300px;
      // box-sizing: border-box;
      height: 18px;
      // padding: 10px ;
      padding: 10px 118px 12px 16px;
    }

    .clear {
      width: 32px;
      height: 100%;
      position: absolute;
      left: 403px;
      top: 0px;
      cursor: pointer;
      img {
        width: 100%;
        height: 100%;
      }
    }
    .sub {
      background-color: #4e6ef2;
      cursor: pointer;
      height: 100%;
      width: 108px;
      position: absolute;
      right: 0px;
      color: #fff;
    }
  }
}
</style>
