<template>
  <div>
    <el-row :gutter="20">
      <el-col :span="8">
        <el-card shadow="hover" class="mgb20" style="height:252px;">
          <div class="user-info">
            <img src="static/img/admin.jpg" class="user-avator" alt="">
            <div class="user-info-cont">
              <div class="user-info-name">{{ userName }}</div>
            </div>
          </div>
          <div class="user-info-list">上次登录时间：<span>2018-01-01</span></div>
          <div class="user-info-list">上次登录地点：<span>成都</span></div>
        </el-card>
        <el-card shadow="hover" style="height:252px;">
          <div slot="header" class="clearfix">
            <span>签单排行榜</span>
          </div>
          王佳丽
          <el-progress :percentage="71.3" color="#42b983"></el-progress>
          聂琪
          <el-progress :percentage="24.1" color="#f1e05a"></el-progress>
          李丽
          <el-progress :percentage="3.7"></el-progress>
          其他
          <el-progress :percentage="0.9" color="#f56c6c"></el-progress>
        </el-card>
      </el-col>
      <el-col :span="16">
        <el-row :gutter="20" class="mgb20">
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-1">
                <i class="el-icon-info grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">100</div>
                  <div>工地总数</div>
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-2">
                <i class="el-icon-success grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">9</div>
                  <div>已竣工</div>
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-3">
                <i class="el-icon-warning grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">80</div>
                  <div>施工中</div>
                </div>
              </div>
            </el-card>
          </el-col>
        </el-row>
        <el-card shadow="hover" style="height:403px;">
          <div slot="header" class="clearfix">
            <span>待办事项</span>
            <el-button style="float: right; padding: 3px 0" type="text">添加</el-button>
          </div>
          <el-table :data="todoList" :show-header="false" height="304" style="width: 100%;font-size:14px;">
            <el-table-column width="40">
              <template slot-scope="scope">
                <el-checkbox v-model="scope.row.status"></el-checkbox>
              </template>
            </el-table-column>
            <el-table-column>
              <template slot-scope="scope">
                <div class="todo-item" :class="{'todo-item-del': scope.row.status}">{{scope.row.title}}</div>
              </template>
            </el-table-column>
            <el-table-column width="60">
              <template slot-scope="scope">
                <i class="el-icon-edit"></i>
                <i class="el-icon-delete"></i>
              </template>
            </el-table-column>
          </el-table>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: 'home',
    data() {
      return {
        todoList: [
          {
            title: '今天要修复100个bug',
            status: false
          },
          {
            title: '今天要修复100个bug',
            status: false
          }
        ]
      }
    },
    computed: {
      userName: {
        get() {
          return this.$store.state.user.name
        }
      }
    },
    created() {},
    mounted(){

    },
    methods: {}
  }
</script>


<style scoped>
  .el-row {
    margin-bottom: 20px;
  }

  .grid-content {
    display: flex;
    align-items: center;
    height: 100px;
  }

  .grid-cont-right {
    flex: 1;
    text-align: center;
    font-size: 14px;
    color: #999;
  }

  .grid-num {
    font-size: 30px;
    font-weight: bold;
  }

  .grid-con-icon {
    font-size: 50px;
    width: 100px;
    height: 100px;
    text-align: center;
    line-height: 100px;
    color: #fff;
  }

  .grid-con-1 .grid-con-icon {
    background: rgb(45, 140, 240);
  }

  .grid-con-1 .grid-num {
    color: rgb(45, 140, 240);
  }

  .grid-con-2 .grid-con-icon {
    background: rgb(100, 213, 114);
  }

  .grid-con-2 .grid-num {
    color: rgb(100, 213, 114);
  }

  .grid-con-3 .grid-con-icon {
    background: rgb(242, 94, 67);
  }

  .grid-con-3 .grid-num {
    color: rgb(242, 94, 67);
  }

  .user-info {
    display: flex;
    align-items: center;
    padding-bottom: 20px;
    border-bottom: 2px solid #ccc;
    margin-bottom: 20px;
  }

  .user-avator {
    width: 120px;
    height: 120px;
    border-radius: 50%;
  }

  .user-info-cont {
    padding-left: 50px;
    flex: 1;
    font-size: 14px;
    color: #999;
  }

  .user-info-cont div:first-child {
    font-size: 30px;
    color: #222;
  }

  .user-info-list {
    font-size: 14px;
    color: #999;
    line-height: 25px;
  }

  .user-info-list span {
    margin-left: 70px;
  }

  .mgb20 {
    margin-bottom: 20px;
  }

  .todo-item {
    font-size: 14px;
  }

</style>
