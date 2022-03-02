<template>
  <el-row :gutter="40" class="panel-group">
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="pageJump('occupational-disease')">
        <div class="card-panel-icon-wrapper icon-sick">
          <svg-icon icon-class="sick-icon" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            法定职业病
          </div>
          <count-to :start-val="0" :end-val="11" :duration="2600" suffix="类" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="pageJump('hf-limite')">
        <div class="card-panel-icon-wrapper icon-danger">
          <svg-icon icon-class="danger" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            危害因素限值
          </div>
          <count-to :start-val="0" :end-val="6" suffix="/358种" :duration="3000" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="pageJump('health-search')">
        <div class="card-panel-icon-wrapper icon-health">
          <svg-icon icon-class="health" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            健康管理自查
          </div>
          <count-to :start-val="0" :end-val="6" suffix="种" :duration="3200" class="card-panel-num" />
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel" @click="pageJump('punish')">
        <div class="card-panel-icon-wrapper icon-arrow">
          <svg-icon icon-class="arrow" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">
            行政处罚条款
          </div>
          <div class="card-panel-num">暂未设计
          </div>
        </div>
      </div>
    </el-col>
  </el-row>
</template>

<script>
import CountTo from 'vue-count-to'

export default {
  components: {
    CountTo
  },
  methods: {
    pageJump(type) {
      this.$emit('pageJump', type)
    }
  }
}
</script>

<style lang="scss" scoped>
.panel-group {
  margin-top: 18px;

  .card-panel-col {
    margin-bottom: 32px;
  }

  .card-panel {
    height: 108px;
    cursor: pointer;
    font-size: 12px;
    position: relative;
    overflow: hidden;
    color: #666;
    background: #fff;
    box-shadow: 4px 4px 40px rgba(0, 0, 0, .05);
    border-color: rgba(0, 0, 0, .05);

    &:hover {
      .card-panel-icon-wrapper {
        color: #fff;
      }

      .icon-sick {
        background: #fd9292;
      }

      .icon-danger {
        background: #36a3f7;
      }

      .icon-health {
        background: #f4516c;
      }

      .icon-arrow {
        background: #34bfa3
      }
    }

    .icon-sick {
      color: #fd9292;
    }

    .icon-danger {
      color: #36a3f7;
    }

    .icon-health {
      color: #f4516c;
    }

    .icon-arrow {
      color: #34bfa3
    }

    .card-panel-icon-wrapper {
      float: left;
      margin: 14px 0 0 14px;
      padding: 12px;
      transition: all 0.38s ease-out;
      border-radius: 6px;
    }

    .card-panel-icon {
      float: left;
      font-size: 48px;
    }

    .card-panel-description {
      float: right;
      font-weight: bold;
      margin: 26px;
      margin-left: 0px;

      .card-panel-text {
        line-height: 18px;
        color: rgba(0, 0, 0, 0.45);
        font-size: 16px;
        margin-bottom: 12px;
      }

      .card-panel-num {
        font-size: 20px;
      }
    }
  }
}

@media (max-width:550px) {
  .card-panel-description {
    display: none;
  }

  .card-panel-icon-wrapper {
    float: none !important;
    width: 100%;
    height: 100%;
    margin: 0 !important;

    .svg-icon {
      display: block;
      margin: 14px auto !important;
      float: none !important;
    }
  }
}
</style>
