<template>
  <div class="hf-limite-wrap">
    <el-card class="box-card" style="margin-bottom:34px">
      <el-form :inline="true" ref="ruleForm" :rules="rules" :model="searchData" class="hf-limite-form">
        <el-form-item label="类别项" prop='type'>
          <el-select v-model="searchData.type" placeholder="请选择">
            <el-option v-for="(item,index) in typeList" :key="'typeList_'+index" :label="item.type" 
            :value="item.typeId"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="自查项目" prop='selfCheck'>
          <el-input v-model="searchData.selfCheck" placeholder="请输入" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSearch('ruleForm')">查询</el-button>
          <el-button @click="onReset('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-card>
    <el-card class="box-card">
      <el-table
        :data="tableData"
        style="width: 100%"
        border
      >
        <el-table-column
          prop="type"
          width='150'
          label="类别项"
          align="center"
        />
        <el-table-column
          prop="selfCheck"
          label="自查项目"
          width='150'
          align="center"
        />
        <el-table-column
          prop="selfCheckContent"
          label="自查内容"
          show-overflow-tooltip
          align="center"
        />
        <el-table-column
          show-overflow-tooltip
          prop="aboutLaw"
          label="相关法律法规"
          align="center"
        />
      </el-table>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchData: {
        type: '', // 类别项
        selfCheck: ''// 自查项目
      },
      rules: {
        selfCheck: [
          { required: true, message: '请输入自查项目', trigger: 'blur' },
        ],
        type: [
          { required: false, message: '请选择类别项', trigger: 'change' }
        ],
      },
      typeList:[
        {
          typeId:1,
          type: '职业病防治管理措施',
        },
        {
          typeId:2,
          type: '职业病危害项目申报',
        },
        {
          typeId:3,
          type: '工作场所职业卫生条件',
        },
        {
          typeId:4,
          type: '职业病危害因素日常监测、检测和评价',
        },
      ],
      tableData: [
        {
          typeId:1,
          type: '职业病防治管理措施',
          selfCheck: '管理机构或者组织',
          selfCheckContent: '设置或者指定职业卫生管理机构或者组织。职业病危害严重或劳动者超过100人的用人单位应设置或者指定职业卫生管理机构或者组织。',
          aboutLaw: '《中华人民共和国职业病防治法》 第二十条 （一）设置或者指定职业卫生管理机构或者组织，配备专职或者兼职的职业卫生管理人员，负责本单位的职业病防治工作；'
        },
        {
          typeId:1,
          type: '职业病防治管理措施',
          selfCheck: '防治计划和实施方案',
          selfCheckContent: '制定年度职业病防治计划和实施方案。',
          aboutLaw: '《中华人民共和国职业病防治法》 第二十条 （二）制定职业病防治计划和实施方案；'
        },
        {
          typeId:2,
          type: '职业病危害项目申报',
          selfCheck: '职业病危害申报',
          selfCheckContent: '工作场所存在职业病目录所列职业病的危害因素的，应当及时、如实向所在地卫生健康主管部门申报危害项目，接受监督。',
          aboutLaw: '《中华人民共和国职业病防治法》 第十六条 用人单位工作场所存在职业病目录所列职业病的危害因素的，应当及时、如实向所在地卫生行政部门申报危害项目，接受监督。'
        },
        {
          typeId:3,
          type: '工作场所职业卫生条件',
          selfCheck: '有害和无害作业分开',
          selfCheckContent: '生产布局合理，符合有害与无害作业分开的原则。',
          aboutLaw: '《中华人民共和国职业病防治法》 第十五条（三）生产布局合理，符合有害与无害作业分开的原则；'
        },
        {
          typeId:4,
          type: '职业病危害因素日常监测、检测和评价',
          selfCheck: '定期检测',
          selfCheckContent: '职业病危害严重的用人单位，应当委托具有相应资质的职业卫生技术服务机构，每年至少进行一次职业病危害因素检测。',
          aboutLaw: '《工作场所职业卫生管理规定》 第二十条 职业病危害严重的用人单位，应当委托具有相应资质的职业卫生技术服务机构，每年至少进行一次职业病危害因素检测，每三年至少进行一次职业病危害现状评价。'
        },
      ]
    }
  },
  methods: {
    onSearch(formName){
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    
                } else {
                    
                    return false;
                }
            });
        },
    onReset(formName){
            this.$refs[formName].resetFields();
    },
  }
}
</script>

<style lang="scss" scoped>
.hf-limite-wrap{
    background:inherit;
    padding: 24px;
    box-sizing: border-box;
    .hf-limite-form{
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
    }
    .el-form-item {
        margin-bottom: 0;
    }
}
</style>
