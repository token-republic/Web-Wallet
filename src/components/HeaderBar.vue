<template>
  <el-aside>
    <div class="logo click" @click="toUrl('home')">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 620 96">
        <defs />
        <g id="Layer_2" data-name="Layer 2">
          <g id="Layer_1-2" data-name="Layer 1">
            <path
              class="cls-1"
              d="M19,2.5A32.77,32.77,0,0,0,2.5,15.08v9.54H33.85L30.23,48.3h7.9L31,79.24,71.61,36.76l-13.8-.24L60,24.62H83.1l9.56-10V2.5Z"
            />
            <text class="cls-2" transform="translate(73.57 79.26)">OKEN NETWORK</text>
          </g>
        </g>
      </svg>
    </div>
    <el-row class="tac">
      <div class="nav">
        <el-menu :default-active="navActives($route.path)" @select="handleSelect">
          <el-menu-item class="font16" index="home">{{$t('nav.wallet')}}</el-menu-item>
          <el-menu-item
            class="font16"
            index="transfer"
            :disabled="addressList.length === 0"
          >{{$t('nav.transfer')}}</el-menu-item>
          <el-menu-item
            class="font16"
            index="consensus"
            :disabled="addressList.length === 0"
          >{{$t('nav.consensus')}}</el-menu-item>
          <el-menu-item
            class="font16"
            index="contract"
            :disabled="addressList.length === 0 || !nodeServiceInfo.isRunSmartContract"
          >{{$t('nav.contracts')}}</el-menu-item>
          <el-menu-item class="font16" index="application" v-show="false">{{$t('nav.application')}}</el-menu-item>
        </el-menu>
      </div>
      <div class="tool">
        <el-menu :default-active="navActive" @select="handleSelect">
          <el-submenu index="address" class="user font16" :disabled="addressList.length === 0">
            <template slot="title">
              <i class="iconfont iconzhanghu"></i>
            </template>
            <el-menu-item v-for="item of addressList" :key="item.address" :index="item.address">
              <span :class="item.selection ? 'fCN' : '' ">
                <i class="iconfont iconwo ico" :class="item.selection ? 'fCN' : 'transparent' "></i>
                <font v-if="item.alias" class="w100">{{item.alias}}</font>
                <font v-else-if="item.remark" class="w100">{{item.remark}}</font>
                <font v-else class="w100">{{item.addresss}}</font> |
                <span>{{item.balance}}</span>
              </span>
            </el-menu-item>
          </el-submenu>
          <el-submenu class="font16" index="set">
            <template slot="title">{{$t('nav.set')}}</template>
            <el-menu-item index="address">{{$t('nav.addressList')}}</el-menu-item>
            <el-menu-item index="nodeService">{{$t('nav.nodeList')}}</el-menu-item>
            <el-menu-item index="contact">{{$t('public.bookList')}}</el-menu-item>
            <el-menu-item index="seting">{{$t('public.about')}}</el-menu-item>
          </el-submenu>

          <!-- <el-menu-item  index="lang">
            <span>{{this.lang ==="en" ? "CN":"EN"}}</span>
          </el-menu-item>

          <el-submenu index="lang">
             <template slot="title">{{this.lang ==="en" ? "Eng":"中文"}}</template>
             <el-menu-item index="cn">中文</el-menu-item>
             <el-menu-item index="en">English</el-menu-item>
          </el-submenu>-->
          <el-submenu class="font16" index="more" v-show="symbol ==='NULS'">
            <template slot="title">
              <i class="el-icon-more"></i>
            </template>
            <el-menu-item index="official">{{$t('tab.tab21')}}</el-menu-item>
            <el-menu-item index="explorer">{{$t('tab.tab22')}}</el-menu-item>
            <el-menu-item index="docs">{{$t('tab.tab23')}}</el-menu-item>
          </el-submenu>
        </el-menu>
      </div>
    </el-row>
  </el-aside>
</template>


<!--
<template>
  <div class="header">
    <div class="w1200">
      <div class="logo">
        <img class="click" @click="toUrl('home')" :src=logoSvg>
      </div>
      <div class="nav">
        <el-menu mode="horizontal" :default-active="navActives($route.path)" @select="handleSelect">
          <el-menu-item index="home">{{$t('nav.wallet')}}</el-menu-item>
          <el-menu-item index="transfer" :disabled="addressList.length === 0">{{$t('nav.transfer')}}
          </el-menu-item>
          <el-menu-item index="consensus" :disabled="addressList.length === 0">{{$t('nav.consensus')}}
          </el-menu-item>
          <el-menu-item index="contract" :disabled="addressList.length === 0 || !nodeServiceInfo.isRunSmartContract">
            {{$t('nav.contracts')}}
          </el-menu-item>
          <el-menu-item index="application" v-show="false">{{$t('nav.application')}}</el-menu-item>
        </el-menu>
      </div>
      <div class="tool">
        <el-menu mode="horizontal" :default-active="navActive" @select="handleSelect">
          <el-submenu index="address" class="user" :disabled="addressList.length === 0">
            <template slot="title"><i class="iconfont iconzhanghu"></i></template>
            <el-menu-item v-for="item of addressList" :key="item.address" :index="item.address">
              <span :class="item.selection ? 'fCN' : '' ">
                 <i class="iconfont iconwo ico" :class="item.selection ? 'fCN' : 'transparent' "></i>
                <font v-if="item.alias" class="w100"> {{item.alias}}</font>
                <font v-else-if="item.remark" class="w100"> {{item.remark}}</font>
                <font v-else class="w100">{{item.addresss}}</font> |
                <span>{{item.balance}}</span>
              </span>
            </el-menu-item>
          </el-submenu>
          <el-submenu index="set">
            <template slot="title">{{$t('nav.set')}}</template>
            <el-menu-item index="address">{{$t('nav.addressList')}}</el-menu-item>
            <el-menu-item index="nodeService">{{$t('nav.nodeList')}}</el-menu-item>
            <el-menu-item index="contact">{{$t('public.bookList')}}</el-menu-item>
            <el-menu-item index="seting">{{$t('public.about')}}</el-menu-item>
          </el-submenu>

          <el-menu-item index="lang">
            <span>{{this.lang ==="en" ? "CN":"EN"}}</span>
          </el-menu-item>

           <el-submenu index="lang">
             <template slot="title">{{this.lang ==="en" ? "Eng":"中文"}}</template>
             <el-menu-item index="cn">中文</el-menu-item>
             <el-menu-item index="en">English</el-menu-item>
           </el-submenu>
          <el-submenu index="more" v-show="symbol ==='NULS'">
            <template slot="title"><i class="el-icon-more"></i></template>
            <el-menu-item index="official">{{$t('tab.tab21')}}</el-menu-item>
            <el-menu-item index="explorer">{{$t('tab.tab22')}}</el-menu-item>
            <el-menu-item index="docs">{{$t('tab.tab23')}}</el-menu-item>
          </el-submenu>
        </el-menu>

      </div>
    </div>
    <div class="cb"></div>
  </div>

</template> -->

<script>
import logo from "@/assets/img/logo.svg";
import logoSvg from "@/assets/img/logo-beta.svg";
import {
  superLong,
  chainIdNumber,
  addressInfo,
  connectToExplorer,
} from "@/api/util";
import { RUN_DEV } from "@/config.js";
export default {
  data() {
    return {
      logoSvg: RUN_DEV ? logo : logoSvg, //logo
      navActive: "/", //菜单选中
      addressList: [], //地址列表
      lang: "cn", //语言选择
      nodeServiceInfo: {},
      symbol: "NULS", //symbol
    };
  },
  components: {},
  created() {
    let type = navigator.appName;
    let langs = "";
    if (type === "Netscape") {
      langs = navigator.language; //获取浏览器配置语言，支持非IE浏览器
    } else {
      langs = navigator.userLanguage; //获取浏览器配置语言，支持IE5+ == navigator.systemLanguage
    }
    let lang = langs.substr(0, 2); //获取浏览器配置语言前两位
    if (lang === "zh") {
      this.lang = "cn";
    } else {
      this.lang = "en";
    }
    this.$i18n.locale = this.lang;
    this.getAddressList();
  },
  mounted() {
    setInterval(() => {
      this.symbol = sessionStorage.hasOwnProperty("info")
        ? JSON.parse(sessionStorage.getItem("info")).defaultAsset.symbol
        : "NULS";
      document.title = this.symbol + " Wallet";
      this.getAddressList();
      if (sessionStorage.hasOwnProperty("info")) {
        this.nodeServiceInfo = JSON.parse(sessionStorage.getItem("info"));
      } else {
        this.nodeServiceInfo.isRunCrossChain = false;
        this.nodeServiceInfo.isRunSmartContract = false;
      }
    }, 500);
  },
  methods: {
    /**
     * 菜单导航
     * @param key
     * @param keyPath
     */
    handleSelect(key, keyPath) {
      if (keyPath.length > 1) {
        if (keyPath[0] === "address") {
          for (let item of this.addressList) {
            //清除选中
            if (item.selection) {
              item.selection = false;
            }
            //添加选中
            if (item.address === keyPath[1]) {
              item.selection = true;
            }
          }
          localStorage.setItem(
            chainIdNumber(),
            JSON.stringify(this.addressList)
          );
        } else if (keyPath[0] === "set") {
          this.$router.push({
            name: keyPath[1],
          });
        } else if (keyPath[0] === "more") {
          let newUrl = "";
          if (keyPath[1] === "official") {
            newUrl = "https://nuls.io/";
          } else if (keyPath[1] === "explorer") {
            newUrl = RUN_DEV
              ? "https://nulscan.io/"
              : "http://beta.nulscan.io/";
          } else if (keyPath[1] === "docs") {
            newUrl = "https://docs.nuls.io/";
          }
          connectToExplorer("nuls", newUrl);
        }
      } else if (key === "lang") {
        this.selectLanguage();
      } else {
        this.$router.push({
          name: key,
        });
      }
    },
    /**
     * 导航栏的选中
     * @param val
     **/
    navActives(val) {
      if (val.indexOf("/transfer") === 0) {
        return "transfer";
      } else if (val.indexOf("/consensus") === 0) {
        return "consensus";
      } else if (val.indexOf("/contract") === 0) {
        return "contract";
      } else {
        return "home";
      }
    },
    /**
     * 获取账户列表
     */
    getAddressList() {
      this.addressList = addressInfo(0);
      if (this.addressList) {
        for (let item of this.addressList) {
          item.addresss = superLong(item.address, 8);
        }
      }
    },
    /**
     * 语言切换
     */
    selectLanguage() {
      if (this.lang === "cn") {
        this.lang = "en";
      } else {
        this.lang = "cn";
      }
      this.$i18n.locale = this.lang;
    },
    /**
     * 连接跳转
     * @param name
     */
    toUrl(name) {
      this.$router.push({
        name: name,
      });
    },
  },
  watch: {},
};
</script>

<style lang="less">
@import "./../assets/css/style";

.cls-1 {
  fill: transparent;
  stroke: #fff;
  stroke-miterlimit: 10;
}
.cls-1 {
  stroke-width: 5px;
}
.cls-2 {
  fill: #fff;
  font-size: 64.96px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
.el-aside {
  background: linear-gradient(
    180deg,
    rgba(35, 42, 52, 1) 40%,
    rgba(0, 168, 204, 1) 100%
  );
  height: 100vh;
  .logo {
    width: 200px;
    margin: 20px auto;
  }
  .nav {
    .el-menu {
      border: 0px;
      background: transparent;
      .is-active {
        font-size: 18px !important;
      }
      .el-menu-item {
        width: 120px;
        border-radius: @Lborder-radius;
        margin: 0 auto;
        color: #fff !important;
        text-align: center;
        background: @BcolorT;
        background-image: linear-gradient(#ffa41b, #ffa41b);
        background-size: 0 3px, auto;
        background-repeat: no-repeat;
        background-position: center bottom;
        transition: all 0.2s ease-out;
        &:hover {
          background-size: 100% 3px, auto;
        }
      }
    }
  }
  .tool {
    .el-menu {
      border: 0px;
      background: @BcolorT;
      .is-disabled{
        background: 0 0!important; 
      }
      .el-submenu {
        
        text-align: center;
        width: 120px;
        border-radius: @Lborder-radius;
        margin: 0 auto;
        background: @BcolorT;
        background-image: linear-gradient(#ffa41b, #ffa41b);
        background-size: 0 3px, auto;
        background-repeat: no-repeat;
        background-position: center bottom;
        transition: all 0.2s ease-out;
        &:hover {
          background-size: 100% 3px, auto;
        }
        .el-submenu__title {
          color: #fff;
           background: @BcolorT;
        background-image: linear-gradient(#ffa41b, #ffa41b);
        background-size: 0 3px, auto;
        background-repeat: no-repeat;
        background-position: center bottom;
        transition: all 0.2s ease-out;
          &:hover{
            background-size: 100% 3px, auto;
          }
          .el-submenu__icon-arrow {
            position: static;
            margin: 0;
            padding-left: 10px;
          }
        }
        .el-menu--inline{
          text-align: center;
          .el-menu-item{
            color: white;
            width: 120px;
            margin: 0 auto;
            padding: 0!important;
          background: @BcolorT;
        background-image: linear-gradient(#ffa41b, #ffa41b);
        background-size: 0 3px, auto;
        background-repeat: no-repeat;
        background-position: center bottom;
        transition: all 0.2s ease-out;
        &:hover {
          background-size: 100% 3px, auto;
        }
          }
          
        }
      }
    }
  }
}
</style>