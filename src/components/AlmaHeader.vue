<template>
  <div class="alma-header">
    <div class="container">
      <div class="logo">Pegasus</div>
      <div class="nav">
        <!-- <div class="nav-item" :class="{ active: $route.name === 'Home' }" @click="jump('Home')">
          首页
        </div>
        <div
          class="nav-item"
          :class="{ active: $route.name === 'SendAirDrop' }"
          @click="jump('SendAirDrop')"
        >
          创建空投
        </div> -->
      </div>
      <div class="op">
        <connect-wallet-btn></connect-wallet-btn>
      </div>
    </div>
  </div>
</template>

<script>
  import { GetSign, GetPublicKey } from 'utils/Provider';
  import ConnectWalletBtn from 'comp/ConnectWalletBtn';
  import { mapGetters } from 'vuex';

  export default {
    data() {
      return {};
    },
    components: {
      ConnectWalletBtn,
    },
    watch: {},
    computed: {
      ...mapGetters(['$accountHash']),
    },
    mounted() {
      // 切链刷新页面
      if (window.starcoin) {
        window.starcoin.on('chainChanged', () => window.location.reload());
      }
    },
    methods: {
      /**
       * 跳转
       */
      jump(routerName) {
        if (this.$route.name !== routerName) {
          this.$router.push({ name: routerName });
        }
      },
    },
  };
</script>

<style lang="less" scoped>
  .alma-header {
    width: 100%;
    background-color: var(--panelBgColor);
    display: flex;
    justify-content: center;

    .container {
      height: 56px;
      display: flex;
      align-items: center;

      .logo {
        font-size: 30px;
        font-weight: bolder;
      }

      .nav {
        display: flex;
        margin: 0 2rem;
        font-size: 14px;

        .nav-item {
          margin-right: 1rem;
          cursor: pointer;
          font-weight: 800;

          &:hover,
          &.active {
            color: var(--mainTextHoverColor);
          }
        }
      }

      .op {
        margin-left: auto;
        display: flex;
        justify-content: space-between;
      }
    }
  }
</style>
