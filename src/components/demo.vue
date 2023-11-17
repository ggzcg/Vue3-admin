<template><div><el-table :row-class-name="tableRowClassName" :data="formLabelAlign" border style="width: 100%"><el-table-column align="center" width="100px" type="index" label="序号"></el-table-column><el-table-column align="center" prop="name" label="编号"><template slot-scope="scope"><el-input size="mini" v-model="scope.row.name"></el-input></template></el-table-column><el-table-column align="center" prop="rylx" label="燃油类型"><template slot-scope="scope"><el-select @change="rylxChange(scope.row.rylx,scope.row.index)" size="mini" v-model="scope.row.rylx" placeholder="请选择燃油类型"><el-option v-for="item in rylxOptions" :key="item.value" :label="item.label" :value="item.value"></el-option></el-select></template></el-table-column><el-table-column align="center" prop="yh" label="油号"><template slot-scope="scope"><el-select size="mini" v-model="scope.row.yh"><el-option v-for="item in scope.row.yhOptions" :key="item.label" :label="item.label" :value="item.label"></el-option></el-select></template></el-table-column><el-table-column align="center" prop="address" label="地址"><template slot-scope="scope"><el-input size="mini" v-model="scope.row.address"></el-input></template></el-table-column><el-table-column align="center" prop="date" label="操作"><template slot-scope="scope"><i @click="addForm" class="el-icon-circle-plus"></i><i v-if="formLabelAlign.length>1" @click="removeIdx(scope.row,scope.row.index)" style="color:rgb(216,30,6)"class="el-icon-remove"></i></template></el-table-column></el-table></div>
</template>

<script>
export default {
  data() {return {
    formLabelAlign: [{name: "",rylx: "",yh: "",address: "",yhOptions: [{value: "1",label: "5",},{value: "2",label: "0",},{value: "3",label: "-10",},{value: "4",label: "-20",},{value: "5",label: "-35",},{value: "6",label: "-50",},],},],rylxOptions: [{value: "0",label: "汽油",},{value: "1",label: "柴油",},],};},
    methods: {// 添加index
      tableRowClassName({ row, rowIndex }) {row.index = rowIndex;},// 切换汽油柴油不同数据
    rylxChange(e, index) {// 柴油
      if (e == "1") {this.formLabelAlign[index].yhOptions = [{value: "1",label: "5",},{value: "2",label: "0",},{value: "3",label: "-10",},{value: "4",label: "-20",},{value: "5",label: "-35",},{value: "6",label: "-50",},];} else {// 汽油
        this.formLabelAlign[index].yhOptions = [{value: "1",label: "89",},{value: "2",label: "92",},{value: "3",label: "95",},{value: "4",label: "98",},];}},// 添加操作
        addForm() {let statusType = true;this.formLabelAlign.forEach((item) => {if (item.name == "" ||item.rylx == "" ||item.yh == "" ||item.address == "") {this.$message({message: "请完善信息后在添加",type: "warning",});statusType = false;}});if (statusType) {this.formLabelAlign.push({name: "",rylx: "",yh: "",address: "",});}},// 删除操作removeIdx(item, index) {console.log(item, index);this.formLabelAlign.splice(index, 1);this.$message({message: "删除成功",type: "success",});},},
}
</script>
<style scoped>
i {font-size: 24px;cursor: pointer;
}
</style>