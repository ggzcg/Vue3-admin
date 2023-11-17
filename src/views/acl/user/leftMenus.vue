<template>
  <div class="menu_box">
    <el-collapse v-model="person.activeName" accordion @change="ParentChange">
      <el-collapse-item
        v-for="(meu, meIndex) of person.menuList"
        :key="meIndex"
        title="标题"
        :name="meIndex + ''"
      >
        <template #title>
          <div
            :class="[
              'sn-menu-title',
              {
                'sn-menu_active':
                  meIndex === Number.parseInt(person.activeName),
              },
              {
                menu_active: person.ParentId === meu.Id,
              },
            ]"
          >
            <div class="sn-big-title">
              <el-icon>
                <OfficeBuilding />
              </el-icon>
              <span>{{ meu.Name }}</span>
            </div>
            <!-- <p v-if="meIndex === 0" class="classs">{{ meu.Name }}</p> -->
          </div>
        </template>
        <p
          :class="[
            'classs',
            {
              menu_active: person.ParentId === meu.Id,
            },
          ]"
          style="border-bottom: 1px solid #e0e0e0"
        >
          {{ meu.Name }}
        </p>
        <div
          class="menu_item"
          v-for="(chi, chiIndex) of meu.Children"
          :key="chiIndex"
          :class="[person.nenuActive === chi.Id ? 'menu_active' : '']"
          @click="menuChange(meu, chi)"
        >
          <div class="childmian">
            <div class="sn-border-top"></div>
            <div class="sn-sub-item">
              <span>{{ chi.Name }}</span>
            </div>
            <div
              class="sn-border-botton"
              v-if="!(chi.Children?.length === chiIndex)"
            ></div>
          </div>
          <div class="childbtn">
            <el-button type="primary" link>copy</el-button>
            <el-button type="primary" link>edit</el-button>
            <el-button type="primary" link>delete</el-button>
          </div>
        </div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>
<script lang="ts" setup>
import { reactive, defineProps } from 'vue'
// import { Document, Menu as IconMenu } from '@element-plus/icons-vue'

let person: any = reactive({
  activeName: '0', // 一级index
  nenuActive: 0, // 二级index
  menuList: [],
})

type Props = {
  menuList: Array<any>
}
const props = withDefaults(defineProps<Props>(), {
  menuList: () => [],
})

const emit = defineEmits(['onToIndex'])
// 父级切换
const ParentChange = (index: number) => {
  emit('onToIndex', person.menuList[index].Id, person.menuList[index].Id)
}
// 子级切换
const menuChange = (meu: any, item: any) => {
  person.nenuActive = item.Id
  person.ParentId = meu.Id
  emit('onToIndex', meu.Id, person.nenuActive)
}

// 刷新后定位
let ParentsId = Number(localStorage.getItem('ParentId'))
let CurrsId = Number(localStorage.getItem('CurrId'))
if (
  JSON.parse(
    JSON.parse(JSON.stringify(window.localStorage.getItem('dataMenu'))),
  )
) {
  person.menuList = JSON.parse(
    JSON.parse(JSON.stringify(window.localStorage.getItem('dataMenu'))),
  )
} else {
  person.menuList = props.menuList
}

if (ParentsId && CurrsId) {
  person.menuList.forEach((me: any, meIndex: number) => {
    if (ParentsId === me.Id) {
      person.activeName = meIndex + ''
      me.Children.forEach((chi: any) => {
        if (CurrsId === chi.Id) {
          person.nenuActive = chi.Id
        }
      })
    }
  })
}
</script>
<style lang="scss" scoped>
.menu_box {
  padding: 20px;
  background: #fff;

  .el-collapse-item__header {
    height: auto;
  }
  .classs {
    // position: absolute;
    // top: 10px;
    margin-left: 15px;
    padding: 12px;
    border-left: 1px solid #e0e0e0;
    // border-bottom: 1px solid #e0e0e0;
  }

  .sn-menu-title {
    .el-icon {
      margin-right: 5px;
      font-size: 18px;
    }
    .sn-big-title {
      display: flex;
      align-items: center;
    }
  }
}
.menu_active {
  color: #6b86ff !important;
}
.menu_item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0px 40px;
  font-size: 14px;
  position: relative;
  .sn-border-top,
  .sn-border-botton {
    height: 25px;
    border-left: 1px solid #e0e0e0;
    position: absolute;
    top: 0;
  }
  .sn-border-botton {
    top: 25px;
  }
  .sn-sub-item {
    display: flex;
    align-items: center;
    background: url('../../assets/img/semicircle.png') no-repeat;
    background-position: 0 50%;
    padding-left: 15px;
    height: 50px;
    line-height: 50px;
    font-size: 15px;
    .el-icon {
      margin: 0px 5px;
      font-size: 16px;
    }
  }
}

.el-collapse {
  width: 320px;
  border: 0;
}

// /deep/ .el-collapse-item__header.is-active {
//   background: #f1f4ff;
//   color: #6b86ff;
//   border-radius: 14px;
// }
// /deep/ .el-collapse-item__header {
//   padding: 5px 10px;
//   font-size: 16px;
//   color: #666;
//   border: 0;
//   height: inherit;
// }
// /deep/ .el-collapse-item__wrap {
//   border: 0;
// }
// /deep/ .el-collapse-item__content {
//   padding: 0;
// }
</style>
