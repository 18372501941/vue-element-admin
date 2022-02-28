<template>
  <div class="app-container">
    <div class="filter-container search">
      <span>职业病种类：</span>
      <el-select v-model="listQuery.specie" placeholder="请选择" filterable clearable style="width: 200px" class="filter-item" @change="handleFilter">
        <el-option v-for="item in speciesOption" :key="item" :label="item" :value="item" />
      </el-select>
      <span>职业病亚类：</span>
      <el-input v-model="listQuery.subSpecie" placeholder="请输入" style="width: 200px;" clearable class="filter-item" @input="handleFilter" />
      <!-- <el-button v-waves :loading="downloadLoading" class="filter-item" type="primary" icon="el-icon-download" @click="handleDownload">
        导出
      </el-button> -->
    </div>

    <el-table
      :data="sickTypeList"
      border
      :row-key="row => { return row.id + row.subSpecies }"
      highlight-current-row
      default-expand-all
      style="width: 100%;"
      height="calc(100vh - 168px)"
      :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
      :header-cell-style="{ background: '#F5F5F5', color: '#9B9B9B' }"
    >
      <el-table-column label="序号" prop="id" align="center" width="80">
        <template slot-scope="{row}">
          <span>{{ row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column label="种类" class-name="status-col">
        <template slot-scope="{row}">
          <span style="font-weight:600">{{ row.species }}</span>
        </template>
      </el-table-column>
      <el-table-column label="亚类" class-name="status-col">
        <template slot-scope="{row}">
          <span>{{ row.subSpecies }}</span>
        </template>
      </el-table-column>
    </el-table>

  </div>
</template>

<script>
export default {
  name: 'SickTable',
  data() {
    return {
      listQuery: {
        specie: '',
        subSpecie: ''
      },
      speciesOption: ['尘肺病', '其他呼吸系统疾病', '职业性皮肤病', '职业性眼病', '职业性耳鼻喉口腔疾病',
        '职业性化学中毒', '物理因素所致职业病', '职业性放射性疾病', '职业性传染病', '职业性肿瘤', '其他职业病'],
      downloadLoading: false,
      sickTypeList: [],
      sickTypeAllList: [
        { id: '一', species: '尘肺病', children: [{
          id: '1',
          subSpecies: '矽肺'
        }, {
          id: '2',
          subSpecies: '煤工尘肺'
        }, {
          id: '3',
          subSpecies: '石墨尘肺'
        }, {
          id: '4',
          subSpecies: '碳黑尘肺'
        }, {
          id: '5',
          subSpecies: '石棉肺'
        }, {
          id: '6',
          subSpecies: '滑石尘肺'
        }, {
          id: '7',
          subSpecies: '水泥尘肺'
        }, {
          id: '8',
          subSpecies: '云母尘肺'
        }, {
          id: '9',
          subSpecies: '陶工尘肺'
        }, {
          id: '10',
          subSpecies: '铝尘肺'
        }, {
          id: '11',
          subSpecies: '电焊工尘肺'
        }, {
          id: '12',
          subSpecies: '铸工尘肺'
        }, {
          id: '13',
          subSpecies: '根据《尘肺病诊断标准》和《尘肺病理诊断标准》可以诊断的其他尘肺'
        }] },
        { id: '二', species: '其他呼吸系统疾病', children: [{
          id: '1',
          subSpecies: '过敏性肺炎'
        }, {
          id: '2',
          subSpecies: '棉尘病'
        }, {
          id: '3',
          subSpecies: '哮喘'
        }, {
          id: '4',
          subSpecies: '金属及其化合物粉尘肺沉着病（锡、铁、锑、钡及其化合物等）'
        }, {
          id: '5',
          subSpecies: '刺激性化学物所致慢性阻塞性肺疾病'
        }, {
          id: '6',
          subSpecies: '金属肺病'
        }] },
        { id: '三', species: '职业性皮肤病', children: [{
          id: '1',
          subSpecies: '接触性皮炎光接触性皮炎'
        }, {
          id: '2',
          subSpecies: '电光性皮炎'
        }, {
          id: '3',
          subSpecies: '黑变病'
        }, {
          id: '4',
          subSpecies: '痤疮溃疡'
        }, {
          id: '5',
          subSpecies: '化学性皮肤灼伤'
        }, {
          id: '6',
          subSpecies: '白斑'
        }, {
          id: '7',
          subSpecies: '根据《职业性皮肤病的诊断总则》可以诊断的其他职业性皮肤病'
        }]
        },
        { id: '四', species: '职业性眼病', children: [{
          id: '1',
          subSpecies: '化学性眼部灼伤'
        }, {
          id: '2',
          subSpecies: '电光性眼炎'
        }, {
          id: '3',
          subSpecies: '白内障（含放射性白内障及三硝基甲苯白内障）'
        }] },
        { id: '五', species: '职业性耳鼻喉口腔疾病', children: [{
          id: '1',
          subSpecies: '噪声聋'
        }, {
          id: '2',
          subSpecies: '铬鼻病'
        }, {
          id: '3',
          subSpecies: '牙酸蚀病'
        }, {
          id: '4',
          subSpecies: '爆震聋'
        }] },
        { id: '六', species: '职业性化学中毒', children: [{
          id: '1',
          subSpecies: '铅及其化合物中毒（不包括四乙基铅）'
        }, {
          id: '2',
          subSpecies: '汞及其化合物中毒'
        }, {
          id: '3',
          subSpecies: '锰及其化合物中毒'
        }, {
          id: '4',
          subSpecies: '镉及其化合物中毒'
        }, {
          id: '5',
          subSpecies: '铍病'
        }, {
          id: '6',
          subSpecies: '铊及其化合物中毒'
        }, {
          id: '7',
          subSpecies: '钡及其化合物中毒'
        }, {
          id: '8',
          subSpecies: '钒及其化合物中毒'
        }, {
          id: '9',
          subSpecies: '磷及其化合物中毒'
        }, {
          id: '10',
          subSpecies: '砷及其化合物中毒'
        }, {
          id: '11',
          subSpecies: '铀及其化合物中毒'
        }, {
          id: '12',
          subSpecies: '砷化氢中毒'
        }, {
          id: '13',
          subSpecies: '氯气中毒'
        }, {
          id: '14',
          subSpecies: '二氧化硫中毒'
        }, {
          id: '15',
          subSpecies: '光气中毒'
        }, {
          id: '16',
          subSpecies: '氨中毒'
        }, {
          id: '17',
          subSpecies: '偏二甲基肼中毒'
        }, {
          id: '18',
          subSpecies: '氮氧化合物中毒'
        }, {
          id: '19',
          subSpecies: '一氧化碳中毒'
        }, {
          id: '20',
          subSpecies: '二硫化碳中毒'
        }, {
          id: '21',
          subSpecies: '硫化氢中毒'
        }, {
          id: '22',
          subSpecies: '磷化氢、磷化锌、磷化铝中毒'
        }, {
          id: '23',
          subSpecies: '氟及其无机化合物中毒'
        }, {
          id: '24',
          subSpecies: '氰及腈类化合物中毒'
        }, {
          id: '25',
          subSpecies: '四乙基铅中毒'
        }, {
          id: '26',
          subSpecies: '有机锡中毒'
        }, {
          id: '27',
          subSpecies: '羰基镍中毒'
        }, {
          id: '28',
          subSpecies: '苯中毒'
        }, {
          id: '29',
          subSpecies: '甲苯中毒'
        }, {
          id: '30',
          subSpecies: '二甲苯中毒'
        }, {
          id: '31',
          subSpecies: '正己烷中毒'
        }, {
          id: '32',
          subSpecies: '汽油中毒'
        }, {
          id: '33',
          subSpecies: '一甲胺中毒'
        }, {
          id: '34',
          subSpecies: '有机氟聚合物单体及其热裂解物中毒'
        }, {
          id: '35',
          subSpecies: '二氯乙烷中毒'
        }, {
          id: '36',
          subSpecies: '四氯化碳中毒'
        }, {
          id: '37',
          subSpecies: '氯乙烯中毒'
        }, {
          id: '38',
          subSpecies: '三氯乙烯中毒'
        }, {
          id: '39',
          subSpecies: '氯丁二烯中毒'
        }, {
          id: '40',
          subSpecies: '苯的氨基及硝基化合物(不包括三硝基甲苯)中毒'
        }, {
          id: '41',
          subSpecies: '三硝基甲苯中毒'
        }, {
          id: '42',
          subSpecies: '酚中毒'
        }, {
          id: '43',
          subSpecies: '五氯酚（钠）中毒'
        }, {
          id: '44',
          subSpecies: '甲醛中毒'
        }, {
          id: '45',
          subSpecies: '硫酸二甲酯中毒'
        }, {
          id: '46',
          subSpecies: '丙烯酰胺中毒'
        }, {
          id: '47',
          subSpecies: '二甲基甲酰胺中毒'
        }, {
          id: '48',
          subSpecies: '有机磷中毒'
        }, {
          id: '49',
          subSpecies: '氨基甲酸酯类中毒'
        }, {
          id: '50',
          subSpecies: '溴甲烷中毒'
        }, {
          id: '51',
          subSpecies: '拟除虫菊酯类中毒'
        }, {
          id: '52',
          subSpecies: '铟及其化合物中毒'
        }, {
          id: '53',
          subSpecies: '溴丙烷中毒'
        }, {
          id: '54',
          subSpecies: '碘甲烷中毒'
        }, {
          id: '55',
          subSpecies: '氯乙酸中毒'
        }, {
          id: '56',
          subSpecies: '环氧乙烷中毒'
        }, {
          id: '57',
          subSpecies: '上述条目未提及的与职业有害因素接触之间存在直接因果联系的其他化学中毒'
        }] },
        { id: '七', species: '物理因素所致职业病', children: [{
          id: '1',
          subSpecies: '中暑'
        }, {
          id: '2',
          subSpecies: '减压病'
        }, {
          id: '3',
          subSpecies: '高原病'
        }, {
          id: '4',
          subSpecies: '航空病'
        }, {
          id: '5',
          subSpecies: '手臂振动病'
        }, {
          id: '6',
          subSpecies: '激光所致眼（角膜、晶状体、视网膜）损伤'
        }, {
          id: '7',
          subSpecies: '冻伤'
        }] },
        { id: '八', species: '职业性放射性疾病', children: [{
          id: '1',
          subSpecies: '外照射急性放射病'
        }, {
          id: '2',
          subSpecies: '外照射亚急性放射病'
        }, {
          id: '3',
          subSpecies: '外照射慢性放射病'
        }, {
          id: '4',
          subSpecies: '内照射放射病'
        }, {
          id: '5',
          subSpecies: '放射性皮肤疾病'
        }, {
          id: '6',
          subSpecies: '放射性肿瘤'
        }, {
          id: '7',
          subSpecies: '放射性骨损伤'
        }, {
          id: '8',
          subSpecies: '放射性甲状腺疾病'
        }, {
          id: '9',
          subSpecies: '放射性性腺疾病'
        }, {
          id: '10',
          subSpecies: '放射复合伤'
        }, {
          id: '11',
          subSpecies: '根据《职业性放射性疾病诊断标准(总则)》可以诊断的其他放射性损伤'
        }] },
        { id: '九', species: '职业性传染病', children: [{
          id: '1',
          subSpecies: '炭疽'
        }, {
          id: '2',
          subSpecies: '森林脑炎'
        }, {
          id: '3',
          subSpecies: '布鲁氏菌病'
        }, {
          id: '4',
          subSpecies: '艾滋病（限于医疗卫生人员及人民警察）'
        }, {
          id: '5',
          subSpecies: '莱姆病'
        }] },
        { id: '十', species: '职业性肿瘤', children: [{
          id: '1',
          subSpecies: '石棉所致肺癌'
        }, {
          id: '2',
          subSpecies: '间皮瘤'
        }, {
          id: '3',
          subSpecies: '联苯胺所致膀胱癌'
        }, {
          id: '4',
          subSpecies: '苯所致白血病'
        }, {
          id: '5',
          subSpecies: '氯甲醚所致肺癌'
        }, {
          id: '6',
          subSpecies: '双氯甲醚所致肺癌'
        }, {
          id: '7',
          subSpecies: '砷及其化合物所致肺癌'
        }, {
          id: '8',
          subSpecies: '皮肤癌'
        }, {
          id: '9',
          subSpecies: '氯乙烯所致肝血管肉瘤'
        }, {
          id: '10',
          subSpecies: '焦炉逸散物所致肺癌'
        }, {
          id: '11',
          subSpecies: '化合物所致肺癌'
        }, {
          id: '12',
          subSpecies: '毛沸石所致肺癌'
        }, {
          id: '13',
          subSpecies: '胸膜间皮瘤'
        }, {
          id: '14',
          subSpecies: '煤油沥青所致皮肤癌'
        }, {
          id: '15',
          subSpecies: '石油沥青所致皮肤癌'
        }, {
          id: '16',
          subSpecies: 'β-萘胺所致膀胱癌'
        }] },
        { id: '十一', species: '其他职业病', children: [{
          id: '1',
          subSpecies: '金属烟热'
        }, {
          id: '2',
          subSpecies: '滑囊炎（限于井下工人）'
        }, {
          id: '3',
          subSpecies: '股静脉血栓综合征'
        }, {
          id: '4',
          subSpecies: '股动脉闭塞症或淋巴管闭塞症（限于刮研作业人员）'
        }] }]
    }
  },
  mounted() {
    this.sickTypeList = [...this.sickTypeAllList]
  },
  methods: {
    handleFilter() {
      const newSickList = []
      const species = this.listQuery.specie
      const subSpecie = this.listQuery.subSpecie
      // 查询条件为空，直接返回结果
      if (!species && !subSpecie) {
        this.sickTypeList = [...this.sickTypeAllList]
      } else {
        for (const ele of this.sickTypeAllList) {
          // 当种类查询条件存在，且和循环中的元素不匹配，跳出循环。
          if (species && ele.species !== species) {
            continue
          }
          const childrenList = []
          for (const eleChild of ele.children) {
            if (eleChild.subSpecies.match(subSpecie)) {
              childrenList.push(eleChild)
            }
          }
          // 成功匹配
          if (childrenList.length) {
            newSickList.push({ id: ele.id, species: ele.species, children: childrenList })
          }
        }
        this.sickTypeList = newSickList
      }
    }
    // handleDownload() {
    //   this.downloadLoading = true
    //   import('@/vendor/Export2Excel').then(excel => {
    //     const tHeader = ['序号', '种类', '亚类']
    //     const filterVal = ['id', 'species', 'children']
    //     const data = this.formatJson(filterVal)
    //     excel.export_json_to_excel({
    //       header: tHeader,
    //       data,
    //       filename: '职业病类型表'
    //     })
    //     this.downloadLoading = false
    //   })
    // },
    // formatJson(filterVal) {
    //   return this.list.map(v => filterVal.map(j => {
    //     if (j === 'timestamp') {
    //       return parseTime(v[j])
    //     } else {
    //       return v[j]
    //     }
    //   }))
    // }
  }
}
</script>
<style lang="scss" scoped>
  .app-container{
    .search{
      font-weight: 600;
      font-size: 16px;
      margin-left: 26px;
      color: #6c6c6c
    }
    .filter-item {
      margin: 0 40px 0 0 ;
    }
  }
</style>
<style lang="scss">
.app-container {
  height: calc(100vh - 93px);
}
</style>
