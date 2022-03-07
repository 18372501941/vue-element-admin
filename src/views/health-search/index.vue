<template>
  <div class="hf-limite-wrap">
    <el-card class="box-card" style="margin-bottom:34px">
      <el-form :inline="true" ref="ruleForm" :rules="rules" :model="searchData" class="hf-limite-form">
        <el-form-item label="类别项" prop='type'>
          <el-select 
            v-model="searchData.type" 
            value-key='typeId'
            @clear="clear('typeId')" 
            @change="typeChange" 
            clearable  
            placeholder="请选择">
            <el-option v-for="item in typeList" :key="item.typeId" :label="item.type" 
            :value="item"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="自查项目" prop='selfCheck'>
          <el-select 
            v-model="searchData.selfCheck" 
            no-data-text='请先选择类别项！' 
            @change="selfCheckChange" 
            clearable 
            value-key='projectId'
            placeholder="请选择" 
            @clear="clear('projectId')">
            <el-option v-for="item in selfCheckList" :key="item.projectId" :label="item.name" 
            :value="item"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSearch('ruleForm')">查询</el-button>
          <el-button @click="onReset('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-card>
    <el-card class="box-card">
      <el-table
        :data="searchTableData"
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
          { required: true, message: '请选择自查项目', trigger: 'change' },
        ],
        type: [
          { required: true, message: '请选择类别项', trigger: 'change' }
        ],
      },
      typeList:[
        {
          typeId:1,
          type: '职业病防治管理措施',
          list:[
            {
              projectId:'1-001',
              name:'管理机构或者组织',
            },
            {
              projectId:'1-002',
              name:'管理人员',
            },
            {
              projectId:'1-003',
              name:'防治计划和实施方案',
            },
          ]
        },
        {
          typeId:2,
          type: '职业病危害项目申报',
          list:[
            {
              projectId:'2-001',
              name:'职业病危害申报',
            },
            {
              projectId:'2-002',
              name:'变更申报',
            },
          ]
        },
        {
          typeId:3,
          type: '工作场所职业卫生条件',
          list:[
            {
              projectId:'3-001',
              name:'有害和无害作业分开',
            },
            {
              projectId:'3-002',
              name:'职业病危害因素浓度或强度',
            },
          ]
        },
        {
          typeId:4,
          type: '职业病危害因素日常监测、检测和评价',
          list:[
            {
              projectId:'4-001',
              name:'定期检测',
            },
          ]
        },
      ],
      selfCheckList:[],
      tableData: [
        {
          typeId:1,
          projectId:'1-001',
          type: '职业病防治管理措施',
          selfCheck: '管理机构或者组织',
          selfCheckContent: '设置或者指定职业卫生管理机构或者组织。职业病危害严重或劳动者超过100人的用人单位应设置或者指定职业卫生管理机构或者组织。',
          aboutLaw: '《中华人民共和国职业病防治法》 第二十条 （一）设置或者指定职业卫生管理机构或者组织，配备专职或者兼职的职业卫生管理人员，负责本单位的职业病防治工作；'
        },
        {
          typeId:1,
          projectId:'1-002',
          type: '职业病防治管理措施',
          selfCheck: '管理人员',
          selfCheckContent: '配备专职或者兼职的职业卫生管理人员。职业病危害严重或劳动者超过100人的企业应配备专职职业卫生管理人员；其他存在职业病危害的用人单位，劳动者在100人以下的，应当配备专职或者兼职职业卫生管理人员。',
          aboutLaw: '《中华人民共和国职业病防治法》 第二十条 （一）设置或者指定职业卫生管理机构或者组织，配备专职或者兼职的职业卫生管理人员，负责本单位的职业病防治工作；《工作场所职业卫生管理规定》第八条 职业病危害严重的用人单位，应当设置或者指定职业卫生管理机构或者组织，配备专职职业卫生管理人员。其他存在职业病危害的用人单位，劳动者超过一百人的，应当设置或者指定职业卫生管理机构或者组织，配备专职职业卫生管理人员；'
        },
        {
          typeId:1,
          projectId:'1-003',
          type: '职业病防治管理措施',
          selfCheck: '防治计划和实施方案',
          selfCheckContent: '制定年度职业病防治计划和实施方案。',
          aboutLaw: '《中华人民共和国职业病防治法》 第二十条 （二）制定职业病防治计划和实施方案；'
        },
        {
          typeId:2,
          projectId:'2-001',
          type: '职业病危害项目申报',
          selfCheck: '职业病危害申报',
          selfCheckContent: '工作场所存在职业病目录所列职业病的危害因素的，应当及时、如实向所在地卫生健康主管部门申报危害项目，接受监督。',
          aboutLaw: '《中华人民共和国职业病防治法》 第十六条 用人单位工作场所存在职业病目录所列职业病的危害因素的，应当及时、如实向所在地卫生行政部门申报危害项目，接受监督。'
        },
        {
          typeId:2,
          projectId:'2-002',
          type: '职业病危害项目申报',
          selfCheck: '变更申报',
          selfCheckContent: '重要事项变化时及时进行变更申报。',
          aboutLaw: `《职业病危害项目申报办法》 第八条 用人单位有下列情形之一的，应当按照本条规定向原申报机关申报变更职业病危害项目内容： 
                    （一）进行新建、改建、扩建、技术改造或者技术引进建设项目的，自建设项目竣工验收之日起30日内进行申报； 
                    （二）因技术、工艺、设备或者材料等发生变化导致原申报的职业病危害因素及其相关内容发生重大变化的，自发生变化之日起15日内进行申报； 
                    （三）用人单位工作场所、名称、法定代表人或者主要负责人发生变化的，自发生变化之日起15日内进行申报； 
                    （四）经过职业病危害因素检测、评价，发现原申报内容发生变化的，自收到有关检测、评价结果之日起15日内进行申报。 
                    `
        },
        {
          typeId:3,
          projectId:'3-001',
          type: '工作场所职业卫生条件',
          selfCheck: '有害和无害作业分开',
          selfCheckContent: '生产布局合理，符合有害与无害作业分开的原则。',
          aboutLaw: '《中华人民共和国职业病防治法》 第十五条（三）生产布局合理，符合有害与无害作业分开的原则；'
        },
        {
          typeId:3,
          projectId:'3-002',
          type: '工作场所职业卫生条件',
          selfCheck: '职业病危害因素浓度或强度',
          selfCheckContent: '工作场所职业病危害因素强度或者浓度符合国家职业卫生标准和行业标准的要求。',
          aboutLaw: '《中华人民共和国职业病防治法》 第十五条（一）职业病危害因素的强度或者浓度符合国家职业卫生标准；'
        },
        {
          typeId:4,
          projectId:'4-001',
          type: '职业病危害因素日常监测、检测和评价',
          selfCheck: '定期检测',
          selfCheckContent: '职业病危害严重的用人单位，应当委托具有相应资质的职业卫生技术服务机构，每年至少进行一次职业病危害因素检测。',
          aboutLaw: '《工作场所职业卫生管理规定》 第二十条 职业病危害严重的用人单位，应当委托具有相应资质的职业卫生技术服务机构，每年至少进行一次职业病危害因素检测，每三年至少进行一次职业病危害现状评价。'
        },
      ],
      searchTableData:[],
    }
  },
  created(){
    this.searchTableData = this.tableData;
  },
  methods: {
    clear(type){
      if(type === 'typeId'){
        this.searchData.selfCheck = '';
        this.onReset('ruleForm');
      }else if(type === 'projectId'){
        let obj = this.searchData.type
        this.searchTableData = obj.typeId ? this.fuzzySearch(this.tableData,obj,'typeId') : this.tableData;
      }
    },
    // 更改自查项目
    selfCheckChange(item){
      let arr = item.typeId ? this.fuzzySearch(this.tableData,item,'typeId') : this.tableData;
      this.searchTableData = item.projectId ? this.fuzzySearch(arr,item,'projectId') : arr;
    },
    // 更改类别项值
    typeChange(item){
      this.searchData.selfCheck = '';
      this.selfCheckList = item.list;
      this.searchTableData = item.typeId ? this.fuzzySearch(this.tableData,item,'typeId') : this.tableData;
    },
    onSearch(formName){
      this.$refs[formName].validate((valid) => {
        if (valid) {
          let arr = this.fuzzySearch(this.tableData,this.searchData.type,'typeId');
          this.searchTableData = this.fuzzySearch(arr,this.searchData.selfCheck,'projectId');
        } else {
          return false;
        }
      });
    },
    onReset(formName){
      this.$refs[formName].resetFields();
      this.searchTableData = this.tableData;
    },
    fuzzySearch(list,obj,str){
        let arr = [];
        list.map(function(item) {
          if(item[str] === obj[str]){
            arr.push(item);
          }
        });
        return arr;
    }
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
