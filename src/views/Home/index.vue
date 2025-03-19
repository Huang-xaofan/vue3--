<template>
  <div class="wrap">
    <div class="tab">服务管理首页</div>
    <div class="card-container">
      <div class="card card1">
        <div class="left">
          <img
            src="../../assets/images/图标1.png"
            alt="服务数量"
            class="icon"
          />
        </div>
        <div class="right">
          <div class="title">服务数量</div>
          <div class="value">268 <span>个</span></div>
        </div>
      </div>
      <div class="card card2">
        <div class="left">
          <img
            src="../../assets/images/图标2.png"
            alt="调用次数"
            class="icon"
          />
        </div>
        <div class="right">
          <div class="title">调用次数</div>
          <div class="value value1">1364 <span class="value1">次</span></div>
        </div>
      </div>
      <div class="card card3">
        <div class="left">
          <img
            src="../../assets/images/图标3.png"
            alt="共享数据"
            class="icon"
          />
        </div>
        <div class="right">
          <div class="title">共享数据</div>
          <div class="value value2">268 <span class="value2">万条</span></div>
        </div>
      </div>
      <div class="card card4">
        <div class="left">
          <img
            src="../../assets/images/图标4.png"
            alt="数据预警"
            class="icon"
          />
        </div>
        <div class="right">
          <div class="title">数据预警</div>
          <div class="value value3">367 <span class="value3">次</span></div>
        </div>
      </div>
    </div>
    <div class="chart">
      <div class="left">
        <lineChart></lineChart>
      </div>

      <div class="right"><pieChart></pieChart></div>
    </div>
    <div class="table">
      <Table></Table>
    </div>
  </div>
</template>

<script setup>
import lineChart from "@/components/lineChart.vue";
import Table from "@/components/table.vue";
import pieChart from "@/components/pieChart.vue";
</script>

<style lang="scss" scoped>
.wrap {
  padding: 20px;
  .tab {
    font-family: Source Han Sans CN;
    font-weight: 500;
    color: #000000;
    font-size: 30px;
  }
  .card-container {
    display: flex;
    justify-content: space-between; // 让左右两侧对齐，中间有间距
    gap: 20px;
    margin: 20px 0;
    .card {
      width: 100%;
      height: 120px;
      display: flex;
      background-size: 100% 100%;
      .left {
        margin: 26px 40px;
        width: 68px;
        height: 68px;
        img {
          width: 100%;
        }
      }
      .right {
        margin: 20px 0;
        .title {
          font-family: Microsoft YaHei;
          font-weight: 700;
          color: #333333;
          font-size: 20px;
          line-height: 27px;
        }
        .value {
          font-weight: 700;
          color: #258dbd;
          font-size: 40px;
          span {
            color: #258dbd;
            font-size: 20px;
          }
        }
        .value1 {
          color: #50a531;
          span {
            color: #50a531;
          }
        }
        .value2 {
          color: #1779d1;
          span {
            color: #1779d1;
          }
        }
        .value3 {
          color: #1b9e80;
          span {
            color: #1b9e80;
          }
        }
      }
    }
    .card1 {
      background-image: url("../../assets/images/服务数量bg.png");
    }
    .card2 {
      background-image: url("../../assets/images/调用次数bg.png");
    }
    .card3 {
      background-image: url("../../assets/images/共享数据bg.png");
    }
    .card4 {
      background-image: url("../../assets/images/数据预警bg.png");
    }
  }
  .chart {
    display: flex;
    justify-content: space-between; // 让左右两侧对齐，中间有间距
    .left {
      width: calc(50% - 10px); // 让两个部分等宽，保证间距对称
    }
    .right {
      width: calc(50% - 10px); // 让两个部分等宽，保证间距对称
    }
  }
  .table {
    margin-top: 26px;
  }
}
</style>
