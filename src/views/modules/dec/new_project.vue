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
              <el-form-item label="工地名称" prop="name">
                <el-input v-model="dataForm.name"></el-input>
              </el-form-item>
              <el-row>
                <el-col :span="12">
                  <el-form-item label="业主姓名" prop="ownerName">
                    <el-input v-model="dataForm.ownerName"></el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="12">
                  <el-form-item label="联系方式" prop="ownerTel">
                    <el-input v-model="dataForm.ownerTel"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="12">
                  <el-form-item label="设计师" prop="designer">
                    <el-select v-model="dataForm.designer" placeholder="请选择" filterable style="width: 100%">
                      <el-option
                        v-for="item in users"
                        :key="item.userId"
                        :label="item.username"
                        :value="item.userId+':'+item.username"
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
                        :key="item.userId"
                        :label="item.username"
                        :value="item.userId+':'+item.username"
                        :disabled="item.disabled">
                      </el-option>
                    </el-select>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="12">
                  <el-form-item label="开始时间" prop="startContractTime">
                    <el-date-picker type="date" placeholder="开始时间" v-model="dataForm.startContractTime"
                                    style="width: 100%;" value-format="timestamp"></el-date-picker>
                  </el-form-item>
                </el-col>
                <el-col :span="12">
                  <el-form-item label="结束时间" prop="endContractTime">
                    <el-date-picker type="date" placeholder="结束时间" v-model="dataForm.endContractTime"
                                    style="width: 100%;" value-format="timestamp"></el-date-picker>
                  </el-form-item>
                </el-col>
              </el-row>
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
    <el-button type="default" @click="back2List">返回</el-button>
  </div>
</template>

<script>
  import SelectMaterialHead from './components/select-material-head';
  import {geneCreateMaterials} from '../../../utils/project';
  import ElRow from "element-ui/packages/row/src/row";
  import {isMobile} from '../../../utils/validate';
  const validMobile=(rule, value,callback)=>{
    if (!value){
      callback(new Error('请输入电话号码'))
    }else  if (!isMobile(value)){
      callback(new Error('请输入正确的11位手机号码'))
    }else {
      callback()
    }
  };

  export default {
    name: "new_project",
    data: function () {
      return {
        users:[],
        dataForm: {
          id: 0,
          name: '',
          ownerName: '',
          ownerTel: '',
          designer: '',
          manager: '',
          startContractTime: undefined,
          endContractTime: undefined,
          remark: ''
        },
        dataRule: {
          name: [
            {required: true, message: '工地名称不能为空', trigger: 'blur'}
          ],
          ownerName: [
            {required: true, message: '业主信息不能为空', trigger: 'blur'}
          ],
          ownerTel: [
            {required: true, trigger: 'blur', validator:validMobile}
          ],
          designer: [
            {required: true, message: '设计师不能为空', trigger: 'blur'}
          ],
          startContractTime: [
            {required: true, message: '开始时间不能为空', trigger: 'change', type: 'date'}
          ],
          endContractTime: [
            {required: true, message: '结束时间不能为空', trigger: 'change', type: 'date'}
          ]
        },
        //选材信息
        materials: {}
      }
    },
    components: {
      ElRow,
      SelectMaterialHead
    },
    computed: {
      menuActiveName: {
        get() {
          return this.$store.state.common.menuActiveName
        },
        set(val) {
          this.$store.commit('common/updateMenuActiveName', val)
        }
      },
      mainTabs: {
        get() {
          return this.$store.state.common.mainTabs
        },
        set(val) {
          this.$store.commit('common/updateMainTabs', val)
        }
      },
      mainTabsActiveName: {
        get() {
          return this.$store.state.common.mainTabsActiveName
        },
        set(val) {
          this.$store.commit('common/updateMainTabsActiveName', val)
        }
      },
    },
    created() {
      this.dataForm.id = this.$route.query.id;
      this.init();
    },
    activated() {
      this.materials = geneCreateMaterials();
      this.getAllUsers()
    },
    methods: {
      init(){
        this.$nextTick(() => {
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/dec/project/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams({type:1})
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.project.name;
                this.dataForm.ownerName = data.project.ownerName;
                this.dataForm.ownerTel = data.project.ownerTel;
                this.dataForm.designer = data.project.designer;
                this.dataForm.manager = data.project.manager;
                this.dataForm.startContractTime = new Date(data.project.startContractTime);
                this.dataForm.endContractTime = new Date(data.project.endContractTime);
                this.dataForm.remark = data.project.remark;
                this.materials = data.project.materials;
              }
            })
          }
        })
      },
      addItem(type) {
        this.materials[type].push({position: '', brand: '', standard: '', modal: '', remark: ''});
      },
      delItem(type, index) {
        this.materials[type].splice(index, 1);
      },
      // tabs, 删除tab
      removeTabHandle(tabName) {
        this.mainTabs = this.mainTabs.filter(item => item.name !== tabName);
        if (this.mainTabs.length >= 1) {
          // 当前选中tab被删除
          if (tabName === this.mainTabsActiveName) {
            let tab = this.mainTabs[this.mainTabs.length - 1];
            this.$router.push({name: tab.name, query: tab.query, params: tab.params}, () => {
              this.mainTabsActiveName = this.$route.name
            })
          }
        } else {
          this.menuActiveName = '';
          this.$router.push({name: 'home'})
        }
      },
      // 获取用户列表
      getAllUsers() {
        this.$http({
          url: this.$http.adornUrl('/sys/user/dropdown/users'),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.users = data.users;
          } else {
            this.users = [];
            this.$message.error(data.msg)
          }
        })
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
                    //跳转到列表页，并关闭当前页
                    this.back2List();
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      back2List() {
        this.removeTabHandle(this.mainTabsActiveName);
        this.$router.push({path: '/dec-project'});
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
