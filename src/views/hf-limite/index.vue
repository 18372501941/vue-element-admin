<template>
    <div class="hf-limite-wrap">
        <el-card class="box-card" style="margin-bottom:34px">
            <el-form :inline="true" ref="ruleForm" :model="searchData" :rules="rules" class="hf-limite-form">
                <el-form-item label="危害因素分类"  prop="classify">
                    <el-select v-model="searchData.classify" @change="changeSelect" placeholder="请选择">
                        <el-option label="化学因素" value="chemistry"></el-option>
                        <el-option label="物理因素" value="physics"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="危害因素名称"  prop="name">
                    <el-input v-model="searchData.name" placeholder="请输入"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="onSearch('ruleForm')">查询</el-button>
                    <el-button  @click="onReset('ruleForm')">重置</el-button>
                </el-form-item>
                </el-form>
        </el-card>
        <!-- table表格 -->
        <el-tabs v-model="activeName" type="border-card">
            <el-tab-pane label="化学因素" name="chemistry">
                <el-table
                v-if="activeName === 'chemistry'"
                :data="searchTableData"
                style="width: 100%">
                <el-table-column
                    prop="classify"
                    label="类别"
                    align='center'
                    >
                    <template slot-scope="scope">
                    <span>{{ scope.row.classify === 'chemistry'?'化学因素':'物理因素' }}</span>
                    </template>
                </el-table-column>
                <el-table-column
                    prop="name"
                    label="中文名"
                    align='center'
                    >
                    <template slot-scope="scope">
                        <el-link @click="checkDetail" type="primary" :underline="false">{{scope.row.name}}</el-link>
                    </template>
                </el-table-column>
                <el-table-column
                    prop="englishName"
                    label="英文名"
                    align='center'
                    >
                </el-table-column>
                <el-table-column
                    prop="number"
                    label="文摘号"
                    align='center'
                >
                </el-table-column>
                <el-table-column align='center' label="职业接触限制OELs（mg/m³）">
                    <el-table-column
                        prop="mac"
                        align='center'
                        label="最高容许浓度(MAC)">
                        <template slot-scope="scope">
                            {{scope.row.oels.mac || '-'}}
                        </template>
                    </el-table-column>
                    <el-table-column
                        prop="pcTwa"
                        align='center'
                        width='170'
                        label="时间加权平均容许浓度(PC-TWA)">
                        <template slot-scope="scope">
                            {{scope.row.oels.pcTwa || '-'}}
                        </template>
                    </el-table-column>
                    <el-table-column
                        prop="pcStel"
                        align='center'
                        width='150'
                        label="短时间接触容许浓度(PC-STEL)">
                        <template slot-scope="scope">
                            {{scope.row.oels.pcStel || '-'}}
                        </template>
                    </el-table-column>
                </el-table-column>
                </el-table>
            </el-tab-pane>
            <el-tab-pane label="物理因素" name="physics">
                <el-table
                    v-if="activeName === 'physics'"
                    :data="searchTableList"
                    border
                    style="width: 100%">
                    <el-table-column
                        prop="classify"
                        label="类别"
                        align='center'
                        >
                        <template slot-scope="scope">
                        <span>{{ scope.row.classify === 'chemistry'?'化学因素':'物理因素' }}</span>
                        </template>
                    </el-table-column>
                    <el-table-column
                        prop="name"
                        label="名称"
                        align='center'
                        >
                    </el-table-column>
                    <el-table-column
                        prop="time"
                        label="接触时间"
                        align='center'
                        >
                    </el-table-column>
                    <el-table-column
                        width='200'
                        prop="speed"
                        label="等能量频率计权振动加速度(m/s²)"
                        align='center'
                        >
                    </el-table-column>
                    <el-table-column
                        prop="frequency"
                        label="频率(f,MHz)"
                        width='100'
                        align='center'
                        >
                    </el-table-column>
                    <el-table-column
                        prop="electricValue"
                        label="电场强度(V/m)"
                        width='120'
                        align='center'
                        >
                    </el-table-column>
                    <el-table-column
                        prop="magneticValue"
                        label="磁场强度(A/m)"
                        width='120'
                        align='center'
                        >
                    </el-table-column>
                </el-table>
            </el-tab-pane>
        </el-tabs> 
        <el-dialog
            title="职业接触水平及其分类控制"
            :visible.sync="dialogVisible"
            width="70%"
            center
            custom-class='hf-limite-dialog'
            >
            <span class="hf-limite-dialog-title">按照劳动者实际接触化学有害因素的水平可将劳动者的接触水平分为5级，与其对应的推荐的控制措施见下表</span>
            <div style="margin-bottom:15px">
                <el-table
                border
                :data="dialogData"
                style="width: 100%">
                <el-table-column
                    prop="level"
                    label="接触等级"
                    align='center'
                    width="180">
                </el-table-column>
                <el-table-column
                    prop="levelDescribe"
                    label="等级描述"
                    align='center'
                    width="180">
                </el-table-column>
                <el-table-column
                    prop="measure"
                    align='center'
                    label="推荐的控制措施">
                </el-table-column>
                </el-table>
            </div>
            <span>
                注：作业管理包括对作业方法、作业时间等制定作业标准，使其标准化；改善作业方法；对作业人员进行指导培
                训以及改善作业条件或工作场所环境等。
            </span>
        </el-dialog>

    </div>
</template>

<script>
export default {
    data(){
        return {
            searchData:{
                name:'',//物理因素
                classify:'',//化学因素
            },
            activeName:'chemistry',
            dialogVisible:false,
            tableData:[
                {
                    name:'安妥',
                    classify:'chemistry',
                    englishName:'Antu',
                    number:'86-88-4',
                    oels:{
                        mac:'',
                        pcTwa:0.3,
                        pcStel:"",
                    },
                },
                {
                    name:'氨',
                    classify:'chemistry',
                    englishName:'Ammonia',
                    number:'7664-41-7',
                    oels:{
                        mac:'',
                        pcTwa:20,
                        pcStel:30,
                    },
                },
                {
                    name:'百草枯',
                    classify:'chemistry',
                    englishName:'Paraquat',
                    number:'4685-14-7',
                    oels:{
                        mac:'',
                        pcTwa:0.5,
                        pcStel:"",
                    },
                },
                
            ],
            tableList:[
                {
                    name:'振动',
                    time:'4h',
                    speed:5,
                    classify:'physics'
                },
                {
                    name:'高频电磁场',
                    frequency:'0.1<=f<3.0',
                    electricValue:50,
                    magneticValue:5,
                    classify:'physics',
                },
                {
                    name:'工频电场',
                    frequency:50,
                    electricValue:5,
                    classify:'physics',
                },
            ],
            dialogData:[
                {
                    level:'0（≤1% OEL）',
                    levelDescribe:'无接触',
                    measure:'不需采取行动',
                },
                {
                    level:'Ⅰ（＞1%，≤10% OEL）',
                    levelDescribe:'接触极低，根据已有信息无相关效应',
                    measure:'一般危害告知，如标签、SDS 等',
                },
                {
                    level:'Ⅱ（＞10%，≤50% OEL）',
                    levelDescribe:'有接触但无明显健康效应',
                    measure:'一般危害告知，特殊危害告知，即针对具体因素的危害进行告知',
                },
                {
                    level:'Ⅲ（＞50%，≤OEL）',
                    levelDescribe:'显著接触，需采取行动限制活动',
                    measure:'一般危害告知、特殊危害告知、职业卫生监测、职业健康监护、作业管理',
                },
                {
                    level:'Ⅳ（＞OEL）',
                    levelDescribe:'超过 OELs',
                    measure:'一般危害告知、特殊危害告知、职业卫生监测、职业健康监护、作业管理、个体防护用品和工程、工艺控制',
                },
            ],
            rules: {
                name: [
                    { required: true, message: '请输入危害因素名称', trigger: 'blur' },
                ],
                classify: [
                    { required: false, message: '请选择危害因素分类', trigger: 'change' }
                ],
            },
            searchTableData:[],
            searchTableList:[],
        }
    },
    created(){
        this.searchTableData = this.tableData;
        this.searchTableList = this.tableList;
    },
    methods:{
        changeSelect(value){
            this.activeName = value;
        },
        fuzzySearch(list,value,str){
            let arr = [];
            list.map(function(item) {
            if (item[str].indexOf(value) !== -1) {
                arr.push(item);
            }
            });
            return arr;
        },
        checkDetail(){
            this.dialogVisible = true
        },
        onSearch(formName){
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    if(this.activeName === 'chemistry'){
                        this.searchTableData = this.fuzzySearch(this.tableData,this.searchData.name,'name');
                    }else{
                        this.searchTableList = this.fuzzySearch(this.tableList,this.searchData.name,'name');
                    }
                    
                } else {
                    
                    return false;
                }
            });
        },
        onReset(formName){
            this.$refs[formName].resetFields();
            this.searchTableData = this.tableData;
            this.searchTableList = this.tableList;
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
.hf-limite-dialog{
    .el-dialog__body{
            padding-top: 10px;
    }
}
</style>
<style lang="scss">
.hf-limite-dialog{
    .el-dialog__body{
            padding-top: 10px;
    }
    .hf-limite-dialog-title{
        display: block;
        margin-bottom: 15px;
    }
}
</style>