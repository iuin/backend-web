<template>
  <div>
    <el-row :gutter="12">
      <el-col :span="6" v-for="(material,index) in materials" :key="index">
        <el-card shadow="hover" style="margin: 5px 0;" :body-style="{ padding: '0px' }">
          <img :src="material.imgUrl" class="image">
          <div style="padding: 10px;text-align: center">
            <span style="font-weight: bold;color: #666666">{{material.name}}</span>
            <div class="bottom clearfix">
              <el-button type="text" class="button" @click="redirect2Items(index)">详情</el-button>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        materials: [],
        dataListLoading: false,
      }
    },
    activated() {
      this.getDataList()
    },
    methods: {
      getDataList() {
        this.dataListLoading = true;
        this.$http({
          url: this.$http.adornUrl('/dec/main_material/list'),
          method: 'get',
          params: this.$http.adornParams({})
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.materials = data.page.list;
          } else {
            this.materials = [];
          }
          this.dataListLoading = false;
        })
      },
      redirect2Items(index) {
        this.$router.push({
          path: '/material_item',
          query: {
            mainId: this.materials[index].id,
            mainName: this.materials[index].name
          }
        });
      }
    }
  }
</script>

<style scoped>
  .image {
    width: 100%;
    height: 150px;
    display: block;
  }

  .bottom {
    margin-top: 5px;
    line-height: 12px;
  }

  .button {
    padding: 0;
    float: right;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }

  .clearfix:after {
    clear: both
  }
</style>
