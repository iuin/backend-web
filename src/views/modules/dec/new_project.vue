<template>
  <div>

    <!--基本信息-->
    <el-row>
      <el-col :span="24">
        <el-card>
          <div slot="header" class="clearfix">
            <span>基本信息</span>
          </div>
          <div style="width: 60%;">
            <el-form ref="dataForm" :rules="dataRule" :model="dataForm" label-width="90px">
              <el-form-item label="工地名称">
                <el-input v-model="dataForm.name"></el-input>
              </el-form-item>
              <el-row>
                <el-col :span="12">
                  <el-form-item label="业主姓名">
                    <el-input v-model="dataForm.ownerName"></el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="12">
                  <el-form-item label="联系方式">
                    <el-input v-model="dataForm.ownerTel"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="12">
                  <el-form-item label="设计师">
                    <el-select v-model="dataForm.designer" placeholder="请选择" filterable style="width: 100%">
                      <el-option
                        v-for="item in users"
                        :key="item.id"
                        :label="item.name"
                        :value="item.id+':'+item.name"
                        :disabled="item.disabled">
                      </el-option>
                    </el-select>
                  </el-form-item>
                </el-col>
                <el-col :span="12">
                  <el-form-item label="工长/PM">
                    <el-select v-model="dataForm.manager" placeholder="请选择" filterable style="width: 100%">
                      <el-option
                        v-for="item in users"
                        :key="item.id"
                        :label="item.name"
                        :value="item.id+':'+item.name"
                        :disabled="item.disabled">
                      </el-option>
                    </el-select>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-form-item label="合同时间">
                <el-col :span="12" style="padding-right: 2px">
                  <el-date-picker type="date" placeholder="开始时间" v-model="dataForm.startContractTime"
                                  style="width: 100%;"></el-date-picker>
                </el-col>
                <el-col :span="12" style="padding-left: 2px">
                  <el-date-picker type="date" placeholder="结束时间" v-model="dataForm.endContractTime"
                                  style="width: 100%;"></el-date-picker>
                </el-col>
              </el-form-item>
              <el-form-item label="备注信息">
                <el-input type="textarea" rows="5" v-model="dataForm.remark"></el-input>
              </el-form-item>
            </el-form>
          </div>
        </el-card>
      </el-col>
    </el-row>
    <br>
    <!--选材信息-->
    <el-row>
      <el-col :span="24">
        <el-card>
          <div slot="header" class="clearfix">
            <span>选材信息</span>
          </div>

          <!--tab标签-->
          <el-tabs style="height: 350px;overflow-y: scroll">
            <el-tab-pane label="地砖">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.brick" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('brick',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('brick')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="木地板">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.floor" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('floor',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('floor')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="门">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.door" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('door',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('door')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="橱柜">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.cupboard" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('cupboard',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('cupboard')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="洁具">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.wc" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('wc',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('wc')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="石材">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.stone" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('stone',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('stone')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="铝扣板/生态木">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.ceiling" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('ceiling',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('ceiling')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="乳胶漆">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.paint" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('paint',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('paint')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="石膏线">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.plaster" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('plaster',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('plaster')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="墙纸">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.wallpaper" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('wallpaper',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('wallpaper')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
            <el-tab-pane label="衣柜">
              <select-material-head></select-material-head>
              <el-row v-for="(item,index) in materials.wardrobe" :key="index">
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.position"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.brand"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.standard"></el-input>
                </el-col>
                <el-col :span="4" style="padding: 2px">
                  <el-input v-model="item.modal"></el-input>
                </el-col>
                <el-col :span="5" style="padding: 2px">
                  <el-input v-model="item.remark"></el-input>
                </el-col>
                <el-col :span="2" style="padding: 2px">
                  <el-button type="warning" circle icon="el-icon-delete" size="mini"
                             @click="delItem('wardrobe',index)"></el-button>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="5" style="padding: 2px">
                  <el-button @click="addItem('wardrobe')">新增栏目</el-button>
                </el-col>
              </el-row>
            </el-tab-pane>
          </el-tabs>
        </el-card>
      </el-col>
    </el-row>
    <br>
    <el-button type="primary" @click="onSubmit">保存</el-button>
  </div>
</template>

<script>
  import SelectMaterialHead from './components/select-material-head';

  export default {
    name: "new_project",
    data: function () {
      return {
        users: [
          {
            id: 1,
            name: '张三',
            disabled: false
          },
          {
            id: 2,
            name: '李四',
            disabled: false
          },
          {
            id: 3,
            name: '王麻子',
            disabled: true
          }
        ],
        dataForm: {
          id:0,
          name: '',
          ownerName: '',
          ownerTel: '',
          designer: '',
          manager: '',
          startContractTime: '',
          endContractTime: '',
          remark: ''
        },
        dataRule: {
          name: [
            {required: true, message: '工地名称不能为空', trigger: 'blur'}
          ],
          ownerName: [
            {required: true, message: '业主信息不能为空', trigger: 'blur'}
          ],
          designer: [
            {required: true, message: '设计师不能为空', trigger: 'blur'}
          ],
          startContractTime: [
            {required: true, message: '合同开始时间不能为空', trigger: 'blur'}
          ],
          endContractTime: [
            {required: true, message: '合同结束时间不能为空', trigger: 'blur'}
          ]
        },
        //选材信息
        materials: {
          brick: [
            {position: '餐客厅', brand: '', standard: '', modal: '', remark: ''},
            {position: '厨房墙砖-地砖', brand: '', standard: '', modal: '', remark: ''},
            {position: '主卫墙砖-地砖', brand: '', standard: '', modal: '', remark: ''},
            {position: '次卫墙砖-地砖', brand: '', standard: '', modal: '', remark: ''},
            {position: '生活阳台墙砖-地砖', brand: '', standard: '', modal: '', remark: ''},
            {position: '门槛石', brand: '', standard: '', modal: '', remark: ''},
            {position: '踢脚线', brand: '', standard: '', modal: '', remark: ''}
          ],
          floor: [
            {position: '餐客厅', brand: '', standard: '', modal: '', remark: ''},
            {position: '卧室', brand: '', standard: '', modal: '', remark: ''}
          ],
          door: [
            {position: '卧室门', brand: '', standard: '', modal: '', remark: ''},
            {position: '厨房门', brand: '', standard: '', modal: '', remark: ''},
            {position: '主卫门', brand: '', standard: '', modal: '', remark: ''},
            {position: '次卫门', brand: '', standard: '', modal: '', remark: ''}
          ],
          cupboard: [
            {position: '台面/柜门/柜体', brand: '', standard: '', modal: '', remark: ''}
          ],
          wc: [
            {position: '花洒（主-次）', brand: '', standard: '', modal: '', remark: ''},
            {position: '蹲便/马桶（主-次）', brand: '', standard: '', modal: '', remark: ''},
            {position: '浴室柜（主-次）', brand: '', standard: '', modal: '', remark: ''}
          ],
          stone: [],
          ceiling: [
            {position: '厨房', brand: '', standard: '', modal: '', remark: ''},
            {position: '主卫', brand: '', standard: '', modal: '', remark: ''},
            {position: '次卫', brand: '', standard: '', modal: '', remark: ''}
          ],
          paint: [
            {position: '1色位置', brand: '', standard: '', modal: '', remark: ''},
            {position: '2色位置', brand: '', standard: '', modal: '', remark: ''},
            {position: '3色位置', brand: '', standard: '', modal: '', remark: ''}
          ],
          plaster: [
            {position: '餐客厅', brand: '', standard: '', modal: '', remark: ''},
            {position: '卧室', brand: '', standard: '', modal: '', remark: ''},
          ],
          wallpaper: [],
          wardrobe: []
        }
      }
    },
    components: {
      SelectMaterialHead
    },
    methods: {
      addItem(type) {
        this.materials[type].push({position: '', brand: '', standard: '', modal: '', remark: ''});
      },
      delItem(type, index) {
        this.materials[type].splice(index, 1);
      },
      onSubmit() {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/dec/project/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'ownerName': this.dataForm.ownerName,
                'ownerTel': this.dataForm.ownerTel,
                'designer': this.dataForm.designer,
                'manager': this.dataForm.manager,
                'startContractTime': this.dataForm.startContractTime,
                'endContractTime': this.dataForm.endContractTime,
                'remark': this.dataForm.remark,
                'materials': this.materials
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    //跳转到列表页
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })

      }
    }
  }
</script>

<style scoped>
  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }

  .clearfix:after {
    clear: both
  }

  el-input {
    border: none;
  }
</style>
