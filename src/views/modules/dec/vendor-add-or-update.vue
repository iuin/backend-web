<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()"
             label-width="80px">
      <el-form-item label="商家名称" prop="companyName">
        <el-input v-model="dataForm.companyName" placeholder="供商名称（合作商家）"></el-input>
      </el-form-item>
      <el-form-item label="联系人" prop="contactName">
        <el-input v-model="dataForm.contactName" placeholder="联系人"></el-input>
      </el-form-item>
      <el-form-item label="电话" prop="contactTel">
        <el-input v-model="dataForm.contactTel" placeholder="联系电话"></el-input>
      </el-form-item>
      <el-form-item label="地址" prop="contactAdd">
        <el-input v-model="dataForm.contactAdd" placeholder="联系地址"></el-input>
      </el-form-item>
      <el-form-item label="状态" prop="status">
        <el-select v-model="dataForm.status" style="width: 100%;">
          <el-option label='有效' value="1">有效</el-option>
          <el-option label="无效" value="0">无效</el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="备注信息" prop="remark">
        <el-input v-model="dataForm.remark" placeholder="备注信息"></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data() {
      return {
        visible: false,
        dataForm: {
          id: 0,
          companyName: '',
          contactName: '',
          contactTel: '',
          contactAdd: '',
          status: '1',
          remark: '',
        },
        dataRule: {
          companyName: [
            {required: true, message: '供商名称（合作商家）不能为空', trigger: 'blur'}
          ],
          contactName: [
            {required: true, message: '联系人不能为空', trigger: 'blur'}
          ],
          contactTel: [
            {required: true, message: '联系电话不能为空', trigger: 'blur'}
          ],
          contactAdd: [
            {required: true, message: '联系地址不能为空', trigger: 'blur'}
          ],
          status: [
            {required: true, message: '业务状态:1有效 0：无效不能为空', trigger: 'blur'}
          ]
        }
      }
    },
    methods: {
      init(id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/dec/vendor/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.companyName = data.vendor.companyName
                this.dataForm.contactName = data.vendor.contactName
                this.dataForm.contactTel = data.vendor.contactTel
                this.dataForm.contactAdd = data.vendor.contactAdd
                this.dataForm.materialIds = data.vendor.materialIds
                this.dataForm.status = data.vendor.status
                this.dataForm.remark = data.vendor.remark
                this.dataForm.createUserId = data.vendor.createUserId
                this.dataForm.createTime = data.vendor.createTime
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit() {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/dec/vendor/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'companyName': this.dataForm.companyName,
                'contactName': this.dataForm.contactName,
                'contactTel': this.dataForm.contactTel,
                'contactAdd': this.dataForm.contactAdd,
                'materialIds': this.dataForm.materialIds,
                'status': this.dataForm.status,
                'remark': this.dataForm.remark,
                'createUserId': this.dataForm.createUserId,
                'createTime': this.dataForm.createTime
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
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
