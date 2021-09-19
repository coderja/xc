<template>
  <div class="audit-record">
    <div class="audit-record-item" v-for="(item, index) in AuditRecord" :key="index">
      <div class="item-status">{{item.name}}</div>
      <div class="item-flex">
        <div>{{item.time}}</div>
        <i class="el-icon-arrow-down" @click="showAuditRecordMore(index)" />
      </div>
      <div class="audit-record-item-more" v-if="item.isShow">
        <p>
          <strong>审核结论：</strong>
          <span class="red">{{item.status}}</span>
        </p>
        <div>
          <strong>审批意见：</strong>
          <p class="approval-comments">{{item.opinion}}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    AuditRecord: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {

    }
  },
  methods: {
    showAuditRecordMore(i) {
      this.AuditRecord[i].isShow = !this.AuditRecord[i].isShow
    }
  }
}
</script>
<style scoped lang="scss">
  .audit-record {
    p {
      margin:0;
      padding: 0;
    }
    .audit-record-item {
      padding: 10px 0;
      overflow: hidden;
      margin: 5px 0;
      position: relative;
      .item-status {
        font-size: 14px;
      }
      .item-flex {
        display: flex;
        justify-content: space-between;
        color: #999;
        margin-top: 5px;
        font-size: 14px;
      }
      .audit-record-item-more {
        padding: 6px 10px;
        color: #666;
        background: #eff0f2;
        margin: 10px 0;
        position: relative;
        z-index:9;
        line-height: 26px;
        font-size: 14px;
        .red {
          color: red;
        }
        &.audit-record-item-more::after {
          width: 0;
          height: 0;
          border-right: 10px solid transparent;
          border-left: 10px solid transparent;
          border-bottom: 10px solid #eff0f2;
          float: right;
          margin-right: 13px;
          position: absolute;
          content: "";
          top: -10px;
          right: 10px;
        }
      }
    }
  }
</style>
