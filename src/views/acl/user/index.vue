/** *@Author: meihang *@Date: 2022/4/20 *@Description:专题录播 */
<template>
  <div class="home_box">
    <LeftMenus
      v-if="recordMenu"
      :menuList="recordMenu"
      @onToIndex="onToIndex"
    ></LeftMenus>
  </div>
</template>

<script lang="ts" setup>
import { reactive } from 'vue'
// import { useRouter } from 'vue-router'
const recordMenu = [
  {
    Id: 1,
    Name: '大福建第一中学扛把子',
    Children: [
      {
        Id: 2,
        Name: '初中1',
        Children: null,
      },
      {
        Id: 12,
        Name: '高中2',
        Children: null,
      },
    ],
  },
  {
    Id: 22,
    Name: '广东第一中学扛把子',
    Children: [
      {
        Id: 23,
        Name: '初中1',
        Children: null,
      },
      {
        Id: 33,
        Name: '高中2',
        Children: null,
      },
    ],
  },
]
import LeftMenus from './leftMenus.vue' //静态引入
let person: any = reactive({
  searchActive: -1, //年级选中
  subjectActive: -1, // 科目选中
  videoActive: 0,
  recordMenu: [], // 录播菜单
  recordId: 0,
  classList: [], // 科目列表
  ParentId: 0,
  CurrId: 0,
  FristId: 0,
})

// 获取左菜单
// 获取点击的id
const onToIndex = (perId: number, currId: number) => {
  person.ParentId = perId
  person.CurrId = currId
  // GetRecordMenu(Number(currId))
  // GetRecordList(currId)
  localStorage.setItem('ParentId', person.ParentId)
  localStorage.setItem('CurrId', person.CurrId)
}
</script>
<style lang="scss" scoped>
.home_box {
  width: 100%;
  display: flex;
}
</style>
