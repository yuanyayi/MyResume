<template>
  <div id="resumeEditor">
    <header>
      <div class="wrap">
        <router-link to="/"><img alt="Vue logo" src="../../assets/logo/logo32.png"/></router-link>
          <router-link to="/resume-editor">My Resume Editor</router-link>
          <nav class="fr">
            <el-button type="primary">保存</el-button>
            <el-button @click="savePDF" type="info">生成PDF</el-button>
          </nav>
      </div>
    </header>
    <main>
      <aside>
        <ul class="editList tabs">
          <el-tooltip class="item" effect="dark" :content="tab.title" placement="right-center" v-for="tab in tabs" :key="tab.label">
            <li :class="{'cur': currentTab === tab.label}" @click="gotoTab(tab.label)">
              <i :class="['fa', 'fa-'+tab.icon]"></i>
            </li>
          </el-tooltip>
        </ul>
        <ul class="editContent">
          <!-- 基本信息 -->
          <li v-show="currentTab === 'BaseInfo'">
            <label>姓名</label>
            <el-input placeholder="请输入姓名" clearable v-model="base.name">
            </el-input>
            <label>期望城市</label>
            <el-input placeholder="希望前往的城市" clearable v-model="base.city">
            </el-input>
            <label>期望职位</label>
            <el-input placeholder="期望从事的职业" clearable v-model="base.position">
            </el-input>
          </li>
          <!-- 职业履历 -->
          <li v-show="currentTab === 'Career'">
            <el-collapse accordion v-model="careerActiveName">
              <el-collapse-item v-for="(c,i) in career" :name="i" :title="c.post +' ['+ c.company+']'">
                <label>日期</label>
                <div class="date-picker-group">
                  <el-date-picker v-model="c.employed" type="daterange" align="center" unlink-panels range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期" format="yyyy 年 MM 月" value-format="yyyy-MM-dd" :picker-options="pickerOptions">
                  </el-date-picker>
                </div>
                <label>公司名称</label>
                <el-input placeholder="公司名称" clearable v-model="c.company">
                </el-input>
                <label>岗位</label>
                <el-input placeholder="岗位" clearable v-model="c.post">
                </el-input>
                <label>职责描述</label>
                <el-input type="textarea" :rows="6" resize="none" placeholder="职责描述" v-model="c.content">
                </el-input>
                <p class="tr" style="margin-top: 10px">
                  <el-button @click="cofirmDeleteCareer(i)" type="danger" icon="el-icon-delete" circle></el-button>
                </p>
              </el-collapse-item>
            </el-collapse>
            <div class="addBtn" @click="addCareer"><i class="el-icon-plus"></i></div>
          </li>
          <!-- 教育经历 -->
          <li v-show="currentTab === 'Study'"></li>
          <!-- 个人项目 -->
          <li v-show="currentTab === 'Projects'"></li>
          <!-- 联系方式 -->
          <li v-show="currentTab === 'Contact'"></li>
        </ul>
      </aside>
      <div id="screen">
        <h2>基本信息</h2>
        <dl>
          <dt>姓名：</dt>
          <dd>{{base.name}}</dd>
          <dt>期望城市：</dt>
          <dd>{{base.city}}</dd>
          <dt>期望职位：</dt>
          <dd>{{base.position}}</dd>
        </dl>
        <h2>职业履历</h2>
        <el-card shadow="hover" v-for="c in career">
          <b>{{c.employed[0]|formatDate}}</b> 至 <b>{{c.employed[1]|formatDate}}</b> <br />
          于 <b>{{c.company}}</b> 任 <b>{{c.post}}</b> 岗<br />
          <b>职责描述：</b>
          <pre class="positionContent">{{c.content}}</pre>
        </el-card>
      </div>
    </main>
  </div>
</template>
<script>
  import html2canvas from "html2canvas"
import * as jsPDF from 'jspdf'

export default {
  name: "resumeEditor",
  data() {
    return {
      // 选项
      currentTab: "BaseInfo",
      tabs: [{
        title: "基本信息",
        icon: "id-badge",
        label: "BaseInfo"
      }, {
        title: "职业履历",
        icon: "briefcase",
        label: "Career"
      }, {
        title: "教育经历",
        icon: "graduation-cap",
        label: "Study"
      }, {
        title: "项目简介",
        icon: "laptop",
        label: "Projects"
      }, {
        title: "联系方式",
        icon: "phone",
        label: "Contact"
      }],
      // 信息
      base: {
        name: "我",
        city: "北京",
        position: "前端开发岗"
      },
      career: [{
        company: "悦动美悦(北京)教育科技有限公司",
        employed: ["2018-07-23", "2019-12-31"],
        post: "前端开发",
        content: "  这是一段乱七八糟的岗位描述。。。这是一段需要换行的工作描述。。。这是一段乱七八糟的岗位描述。。。这是一段需要换行的工作描述。。。这是一段乱七八糟的岗位描述。。。这是一段需要换行的工作描述。。。"
      },{
        company: "必胜课教育科技有限公司",
        employed: ["2016-10-17", "2018-02-10"],
        post: "前端开发",
        content: "  这是一段乱七八糟的岗位描述。。。这是一段需要换行的工作描述。。。这是一段乱七八糟的岗位描述。。。这是一段需要换行的工作描述。。。这是一段乱七八糟的岗位描述。。。这是一段需要换行的工作描述。。。"
      }],
      // 日期选择设置项
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        },
        shortcuts: [{
            text: '至今',
            onClick(picker) {
              picker.$emit('pick', new Date());
            }
          }]
      },
      careerActiveName: 0
    }
  },
  mounted(){
    this.career.sort((c1,c2)=>{
      return c1.employed[1]>c2.employed[1]
    })
  },
  methods: {
    gotoTab(i) {
      this.currentTab = i
    },
    savePDF() {
      const _this = this
      html2canvas(document.getElementById("screen")).then(function(canvas) {
        var imgData = canvas.toDataURL('image/jpeg');
        var doc = new jsPDF("p", "mm", "a4");
        doc.addImage(imgData, 'JPEG', 0, 0, 210, 297);
        doc.save(`${_this.base.name}的PDF简历.pdf`);
      });
    },
    cofirmDeleteCareer(index){
      this.$confirm('确定删除这段经历吗?', {
          distinguishCancelAndClose: true,
          confirmButtonText: '删掉',
          cancelButtonText: '算了不删了',
          type: 'warning'
        }).then(() => {
          this.career.splice(index,1);
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(action => {
          action === 'cancel' ? 
          this.$message({
            type: 'info',
            message: '已取消删除'
          }) : console.log(action);
        });
    },
    addCareer(){
      let today = new Date()
      this.career.unshift({
        company: "",
        employed: ["", `${today.getFullYear()}-${today.getMonth()+1}-${today.getDate()}`],
        post: "前端开发",
        content: ""
      })
      this.careerActiveName = 0
    }
  },
  filters: {
    formatDate(dateStr) {
      let today = new Date();
      let tempDate = new Date()
      let str = dateStr.split(/[^0-9]/);
      tempDate.setFullYear(str[0])
      tempDate.setMonth(str[1] - 1)
      tempDate.setDate(str[2])
      if (tempDate >= today) {
        return '至今'
      }
      return dateStr.slice(0,7)
    }
  }
}

</script>
<style type="text/css">
.el-collapse-item__header {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

</style>
<style lang="less" scoped>
#resumeEditor {
  height: 100vh;
}
header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 9;
  background-color: #fff;
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
.fr {
  float: right;
}
.cf:after {
  content: "";
  display: block;
  height: 0;
  visibility: hidden;
  clear: both;
}
.tr {
  text-align: right;
}
main {
  box-sizing: border-box;
  height: 100vh;
  background-color: #f5f5f5;
  padding: 63px 20px 20px;
  &>* {
    background-color: #fff;
    box-shadow: 1px 1px 3px 0 rgba(0, 0, 0, 0.25);
  }
  display: flex;
  aside {
    flex: 0 0 400px;
    display: flex;
    align-items: stretch;
    overflow-x: hidden;
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
      flex: 1 1 auto;
      li {
        margin: 20px;
        &>* {
          display: block;
        }
      }
      label {
        height: 40px;
        line-height: 40px;
      }
      .addBtn {
        width: 100%;
        height: 48px;
        line-height: 48px;
        text-align: center;
        border: 1px solid #ebeef5;
        border-top: none;
        cursor: pointer;
      }
      .el-date-editor.el-date-editor--daterange {
        width: auto;
      }
      .date-picker-group {
        text-align: justify;
      }
    }
  }
  #screen {
    margin-left: 15px;
    flex: 1;
    overflow-x: hidden;
    padding: 20px;
  }
}

#screen {
  font-size: 14px;
  &>* {
    margin: 10px;
  }
  h2, h3, h4, h5 {
    font-weight: 600;
    margin: 0;
  }
  h2 {
    font-size: 20px;
  }
  h3 {
    font-size: 18px;
  }
  h4 {
    font-size: 16px;
  }
  h5 {
    font-size: 14px;
  }
  // 列表
  dl {
    display: flex;
    flex-wrap: wrap;
    dt {
      flex: 0 0 auto;
    }
    dd {
      flex: 2 1 auto;
    }
  }
  .positionContent {
    width: 100%;
    white-space: pre-wrap;
    word-wrap: break-word;
  }
}

</style>
