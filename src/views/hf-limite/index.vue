<template>
    <div class="hf-limite-wrap">
        <el-card class="box-card" style="margin-bottom:34px">
            <el-form :inline="true" :model="searchData" class="hf-limite-form">
                <el-form-item label="物理因素">
                    <el-input v-model="searchData.physics" placeholder="请输入物理因素"></el-input>
                </el-form-item>
                <el-form-item label="化学因素">
                    <el-input v-model="searchData.chemistry" placeholder="请输入化学因素"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="onSearch">查询</el-button>
                    <el-button  @click="onReset">重置</el-button>
                </el-form-item>
                </el-form>
        </el-card>
        <el-card class="box-card">
            <el-table
            :data="tableData"
            style="width: 100%">
            <el-table-column
            label="序号"
            type="index"
            align='center'
            >
            </el-table-column>
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
            <el-table-column
                prop="pcTwa"
                align='center'
                label="PC-TWA">
                <template slot-scope="scope">
                <span v-if="scope.row.classify === 'physics'">
                    总尘:{{scope.row.pcTwa.allDust || '-'}}mg/m³
                    呼尘:{{scope.row.pcTwa.lungDust || '-'}}mg/m³
                </span>
                <span v-if="scope.row.classify === 'chemistry'">
                    {{scope.row.pcTwa || '-'}}
                </span>
                </template>
            </el-table-column>
            </el-table>
        </el-card>
    </div>
</template>

<script>
export default {
    data(){
        return {
            searchData:{
                physics:'',//物理因素
                chemistry:'',//化学因素
            },
            tableData:[
                {
                    name:'安妥',
                    classify:'chemistry',
                    englishName:'Antu',
                    number:'86-88-4',
                    pcTwa:'0.3',
                },
                {
                    name:'氨',
                    classify:'chemistry',
                    englishName:'Ammonia',
                    number:'7664-41-7',
                    pcTwa:'20',
                },
                {
                    name:'百草枯',
                    classify:'chemistry',
                    englishName:'Paraquat',
                    number:'4685-14-7',
                    pcTwa:'0.5',
                },
                {
                    name:'白云石粉尘',
                    classify:'physics',
                    englishName:'Dolomite dust',
                    number:'-',
                    pcTwa:{
                        allDust:8,
                        lungDust:4,
                    },
                },
                {
                    name:'茶尘',
                    classify:'physics',
                    englishName:'Tea dust',
                    number:'-',
                    pcTwa:{
                        allDust:2,
                        lungDust:'',
                    },
                },
                {
                    name:'玻璃钢粉尘',
                    classify:'physics',
                    englishName:'Fiberglass reinforced plastic dust',
                    number:'-',
                    pcTwa:{
                        allDust:3,
                        lungDust:'',
                    },
                },
            ]
        }
    },
    methods:{
        onSearch(){

        },
        onReset(){

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