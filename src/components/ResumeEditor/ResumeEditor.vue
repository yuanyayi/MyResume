<template>
  <div>
    <header>
      <div class="wrap">
        <router-link to="/"><img alt="Vue logo" src="../../assets/logo/logo32.png"></router-link>
          <router-link to="/resume-editor">My Resume Editor</router-link>
          <nav class="right">
            <el-button type="primary">保存</el-button>
            <el-button type="info">预览</el-button>
          </nav>
      </div>
    </header>
    <main>
      <aside>
        <ul class="editList tabs">
          <li v-for="(tab,i) in tabs" :key="i" :title="tab.title" :class="{'cur': currentTabIndex === i}" @click="gotoTab(i)"><i :class="['fa', 'fa-'+tab.icon]"></i></li>
        </ul>
        <ul class="editContent">
          <li v-show="currentTabIndex === 0">
            <label for="username">姓名</label>
            <el-input placeholder="请输入姓名" clearable v-model="base.name">
            </el-input>
            <label for="username">期望城市</label>
            <el-input placeholder="希望前往的城市" clearable v-model="base.city">
            </el-input>
            <label for="username">期望职位</label>
            <el-input placeholder="期望从事的职业" clearable v-model="base.position">
            </el-input>
          </li>
          <li v-show="currentTabIndex === 1"></li>
          <li v-show="currentTabIndex === 2"></li>
          <li v-show="currentTabIndex === 3"></li>
          <li v-show="currentTabIndex === 4"></li>
        </ul>
      </aside>
      <div class="screen">
        姓名： {{base.name}}
        期望城市：{{base.city}}
        期望职位：{{base.position}}
      </div>
    </main>
  </div>
</template>
<script>
export default {
  name: "resumeEditor",
  data() {
    return {
      currentTabIndex: 0,
      tabs: [{
        label: "基本信息",
        icon: "id-badge"
      }, {
        label: "职业履历",
        icon: "briefcase"
      }, {
        label: "教育经历",
        icon: "graduation-cap"
      }, {
        label: "项目简介",
        icon: "laptop"
      }, {
        label: "联系方式",
        icon: "phone"
      }],
      base: {
        name: "",
        city: "北京",
        position: "前端开发岗"
      }

    }
  },
  methods: {
    gotoTab(i) {
      this.currentTabIndex = i
    }
  }
}

</script>
<style lang="less" scoped>
header {
  .wrap {
    height: 40px;
    line-height: 40px;
    padding: 0 10px;
    &>*+* {
      margin-left: 20px;
    }
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.25);
    a, img {
      vertical-align: middle;
    }
  }
  padding-bottom: 3px;
  .el-button {
    padding: 6px 10px;
  }
}
.right {
  float: right
}
main {
  background-color: #f5f5f5;
  padding: 20px;
  &>* {
    background-color: #fff;
  }
  display: flex;
  aside {
    flex: 1;
    display: flex;
    .tabs {
      width: 60px;
      flex: 0 0 auto;
      background-color: #333;
      li {
        text-align: center;
        padding: 20px 0;
        color: #fff;
        i {
          font-size: 28px;
          vertical-align: middle;
        }
        &:hover {
          background-color: #000;
        }
        &.cur {
          background-color: #fff;
          color: #333;
        }
      }
    }
    .editContent {
      li {
        padding: 20px;
        label {
          display: block;
          float: left;
          height: 40px;
          line-height: 40px;
        }
        .el-input {
          display: block;
          float: left;
        }
      }
    }
  }
  .screen {
    margin-left: 15px;
    flex: 2;
    padding: 10px;
  }
}

</style>
