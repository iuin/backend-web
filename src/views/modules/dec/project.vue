<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item label="项目名称">
        <el-input v-model="dataForm.name" placeholder="项目名称"></el-input>
      </el-form-item>
      <el-form-item label="设计师">
        <el-input v-model="dataForm.designerName" placeholder="设计师"></el-input>
      </el-form-item>
      <el-form-item label="工长/PM">
        <el-input v-model="dataForm.managerName" placeholder="项目经理"></el-input>
      </el-form-item>
      <el-form-item label="状态">
        <el-select v-model="dataForm.status">
          <el-option label="全部" value="-999">全部</el-option>
          <el-option label='未开工' value="0">未开工</el-option>
          <el-option label='施工中' value="1">施工中</el-option>
          <el-option label="已竣工" value="2">已竣工</el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <el-button v-if="isAuth('dec:project:delete')" type="danger" @click="deleteHandle()"
                   :disabled="dataListSelections.length <= 0">批量删除
        </el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <el-table-column
        header-align="center"
        align="left"
        label="工地名称">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="redirect2View(scope.row.id)">{{scope.row.name}}</el-button>
        </template>
      </el-table-column>
      <el-table-column
        header-align="center"
        align="center"
        width="90"
        label="状态">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.status === 0" size="small" type="info">未开工</el-tag>
          <el-tag v-if="scope.row.status === 1" size="small" type="warning">施工中</el-tag>
          <el-tag v-if="scope.row.status === 2" size="small" type="success">已完工</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="ownerName"
        header-align="center"
        align="center"
        label="业主姓名">
      </el-table-column>
      <el-table-column
        prop="ownerTel"
        header-align="center"
        align="center"
        label="业主电话">
      </el-table-column>
      <el-table-column
        prop="designerName"
        header-align="center"
        align="center"
        label="设计师">
      </el-table-column>
      <el-table-column
        prop="managerName"
        header-align="center"
        align="center"
        label="工长">
      </el-table-column>
      <el-table-column
        prop="remark"
        header-align="center"
        align="left"
        label="备注信息">
      </el-table-column>
      <el-table-column
        prop="createTime"
        header-align="center"
        align="center"
        label="创建时间">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="modifyStatus(scope.row.id,scope.row.name, 1, '开工')"
                     v-if="scope.row.status===0">开工
          </el-button>
          <el-button type="text" size="small" @click="modifyStatus(scope.row.id,scope.row.name, 2, '竣工')"
                     v-if="scope.row.status===1">竣工
          </el-button>
          <el-button type="text" size="small" @click="modifyStatus(scope.row.id,scope.row.name, 1, '返工')"
                     v-if="scope.row.status===2">返工
          </el-button>
          <el-button type="text" size="small" @click="redirect2Modify(scope.row.id)">修改</el-button>
          <el-button type="text" size="small" @click="deleteHandle(scope.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
  </div>
</template>

<script>
  export default {
    name: "project",
    data() {
      return {
        dataForm: {
          name: '',
          designerName: '',
          managerName: '',
          status: '-999'
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: []
      }
    },
    activated() {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList() {
        this.dataListLoading = true;
        this.$http({
          url: this.$http.adornUrl('/dec/project/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize,
            'name': this.dataForm.name,
            'designerName': this.dataForm.designerName,
            'managerName': this.dataForm.managerName,
            'status': this.dataForm.status,
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.page.list
            this.totalPage = data.page.totalCount
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      sizeChangeHandle(val) {
        this.pageSize = val;
        this.pageIndex = 1;
        this.getDataList();
      },
      // 当前页
      currentChangeHandle(val) {
        this.pageIndex = val;
        this.getDataList()
      },
      // 多选
      selectionChangeHandle(val) {
        this.dataListSelections = val
      },
      // 删除
      deleteHandle(id) {
        let ids = id ? [id] : this.dataListSelections.map(item => {
          return item.id
        });
        this.$confirm(`确定对[id=${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/dec/project/delete'),
            method: 'post',
            data: this.$http.adornData(ids, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        })
      },
      redirect2View(id) {
        this.$router.push({
          path: '/dec-view_project',
          query: {
            id: id
          }
        });
      },
      redirect2Modify(id) {
        this.$router.push({
          path: '/dec-new_project',
          query: {
            id: id
          }
        });
      },
      modifyStatus(id, name, status, operateMsg) {
        this.$confirm('此操作将对工地:[' + name + ']进行' + operateMsg + '操作, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl(`/dec/project/modifyStatus/${id}`),
            method: 'get',
            params: this.$http.adornParams({'status': status})
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        });
      }
    }
  }
</script>
