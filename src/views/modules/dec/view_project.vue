<template>
  <div>
    <el-row>
      <el-col :span="1.5" class="plr5">
        <el-button type="primary" size="mini" v-print="'#printArea'">打印</el-button>
      </el-col>
      <el-col :span="1.5" class="plr5">
        <el-button type="default" size="mini" @click="back2List">返回</el-button>
      </el-col>
      <el-col :span="21">
        <el-steps :active="project.status" finish-status="success">
          <el-step title="未开工"></el-step>
          <el-step title="施工中"></el-step>
          <el-step title="已完工"></el-step>
        </el-steps>
      </el-col>
    </el-row>
    <div id="printArea">
      <div style="text-align: center;font-weight: bold;font-size: large;padding: 5px;">{{project.name}}选材信息表</div>
      <el-row>
        <el-col :span="24">
          <table style="width: 100%;" class="gridtable">
            <tr>
              <td colspan="2" align="center"><span>{{project.startContractTime}}至{{project.endContractTime}}</span>
              </td>
              <td colspan="2" align="center">业主：{{project.ownerName}}({{project.ownerTel}})</td>
              <td colspan="2" align="center">设计师：<span style="font-weight: bold">{{project.designer}}</span>&nbsp;&nbsp;工长：<span
                style="font-weight: bold">{{project.manager}}</span></td>
            </tr>
            <tr>
              <th width="15%">主材名称</th>
              <th width="20%">位置</th>
              <th width="15%">品牌</th>
              <th width="15%">规格</th>
              <th width="15%">型号</th>
              <th width="20%">备注</th>
            </tr>
            <tr v-for="(item,index) in project.materials.brick">
              <td :rowspan="project.materials.brick.length" v-if="index===0" style="text-align: center">地砖</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.floor">
              <td :rowspan="project.materials.floor.length" v-if="index===0" style="text-align: center">木地板</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.door">
              <td :rowspan="project.materials.door.length" v-if="index===0" style="text-align: center">门</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.cupboard">
              <td :rowspan="project.materials.cupboard.length" v-if="index===0" style="text-align: center">橱柜</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.wc">
              <td :rowspan="project.materials.wc.length" v-if="index===0" style="text-align: center">洁具</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.stone">
              <td :rowspan="project.materials.stone.length" v-if="index===0" style="text-align: center">石材</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.ceiling">
              <td :rowspan="project.materials.ceiling.length" v-if="index===0" style="text-align: center">铝扣板/生态木</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.paint">
              <td :rowspan="project.materials.paint.length" v-if="index===0" style="text-align: center">乳胶漆</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.plaster">
              <td :rowspan="project.materials.plaster.length" v-if="index===0" style="text-align: center">石膏线</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.wallpaper">
              <td :rowspan="project.materials.wallpaper.length" v-if="index===0" style="text-align: center">墙纸</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
            <tr v-for="(item,index) in project.materials.wardrobe">
              <td :rowspan="project.materials.wardrobe.length" v-if="index===0" style="text-align: center">衣柜</td>
              <td>{{item.position}}</td>
              <td>{{item.brand}}</td>
              <td>{{item.standard}}</td>
              <td>{{item.modal}}</td>
              <td>{{item.remark}}</td>
            </tr>
          </table>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
  export default {
    name: "view_project",
    data() {
      return {
        id: 0,
        project: {
          materials: {//主要是控制台报错（warn）
          }
        }
      }
    },
    created() {
      this.id = this.$route.query.id;
    },
    mounted() {
      this.query();
    },
    methods: {
      //这里面可以根据获取到的id进行获取相应数据
      query() {
        this.$nextTick(() => {
          this.$http({
            url: this.$http.adornUrl(`/dec/project/info/${this.id}`),
            method: 'get',
            params: this.$http.adornParams({type: 0})
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.project = data.project;
            }
          })
        })
      },
      back2List() {
        this.$router.push({path: '/dec-project'});
      }
    }
  }
</script>

<style scoped>
  .plr5 {
    padding: 0 5px;
  }

  table.gridtable {
    font-family: verdana, arial, sans-serif;
    font-size: 11px;
    color: #333333;
    border-width: 1px;
    border-color: #666666;
    border-collapse: collapse;
  }

  table.gridtable th {
    border-width: 1px;
    padding: 8px;
    border-style: solid;
    border-color: #666666;
  }

  table.gridtable td {
    border-width: 1px;
    padding: 8px;
    border-style: solid;
    border-color: #666666;
    background-color: #ffffff;
  }
</style>
