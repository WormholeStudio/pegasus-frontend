<template>
  <div class="container home">
    <el-card class="top-card">
      <div class="card-container">
        <div class="card-item">
          <div class="title">待领取的空投</div>
          <div class="card-number">2000.01STC</div>
        </div>
        <el-divider direction="vertical"></el-divider>
        <div class="card-item">
          <div class="title">待领取的空投</div>
          <div class="card-number">2000.01STC</div>
        </div>
        <el-divider direction="vertical"></el-divider>
        <div class="card-item">
          <div class="title">待领取的空投</div>
          <div class="card-number">2000.01STC</div>
        </div>
      </div>
    </el-card>

    <el-card>
      <el-table stripe :data="projectData" empty-text=" " v-loading="tableLoading">
        <el-table-column label="项目" prop="name" width="160"></el-table-column>
        <el-table-column
          label="获奖人数"
          align="right"
          prop="winners_count"
          width="100"
        ></el-table-column>
        <el-table-column label="空投总量" align="right" prop="total_amount"></el-table-column>
        <el-table-column label="开始时间" align="right" prop="start_at" width="130">
          <template slot-scope="scope">
            <div v-if="scope.row.start_at">
              {{ scope.row.start_at | day }}
            </div>
          </template>
        </el-table-column>
        <el-table-column label="结束时间" align="right" prop="end_at" width="130">
          <template slot-scope="scope">
            <div v-if="scope.row.end_at">
              {{ scope.row.end_at | day }}
            </div>
          </template>
        </el-table-column>
        <el-table-column label="状态" align="right" width="120">
          <template slot-scope="scope">
            <state-btn :data="scope.row" @success="getProjectData"></state-btn>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex';
  import StateBtn from 'comp/StateBtn';

  export default {
    name: 'Home',
    data() {
      return {
        tableLoading: false,
        projectData: [],
      };
    },
    components: {
      StateBtn,
    },
    computed: {
      ...mapGetters(['$accountHash']),
    },
    watch: {
      $accountHash() {
        this.getProjectData();
      },
    },
    created() {
      this.getProjectData();
    },
    methods: {
      /**
       * Get Project Data
       */
      getProjectData() {
        this.$service
          .V1AirdropList({
            address: this.$accountHash,
          })
          .then((res) => {
            this.projectData = res;
          });
      },
    },
  };
</script>

<style lang="less" scoped>
  .home {
    padding: 5px 0;

    .top-card {
      margin: 20px 0;
    }

    .card-container {
      display: flex;
      justify-content: center;
      padding: 10px 0;

      .card-item {
        flex: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;

        .title {
          font-size: 14px;
          line-height: 3;
        }

        .card-number {
          font-size: 24px;
        }
      }
    }
  }
</style>
