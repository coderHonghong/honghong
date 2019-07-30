<template>
    <el-dialog :visible.sync="dialog" :title="isAdd ? '新增用户' : '编辑用户'" append-to-body width="960px">
        <el-form ref="form" :inline="true" :model="form" :rules="rules" size="small" label-width="94px">
            <div>
                <div class="form-area-title">使用信息</div>
                <div>
                    <el-form-item label="人员姓名" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="领用日期" prop="">
                        <el-date-picker
                                v-model="form.date"
                                type="date"
                                placeholder="选择日期">
                        </el-date-picker>
                    </el-form-item>
                </div>
            </div>
            <div>
                <div class="form-area-title">基本信息</div>
                <div>
                    <el-form-item label="资产编码" prop="" >
                        <el-input v-model="form.username" disabled/>
                    </el-form-item>
                    <el-form-item label="资产名称" prop="">
                        <el-input v-model="form.username" placeholder="请输入资产名称"/>
                    </el-form-item>
                    <el-form-item label="资产分类" prop="">
                        <el-select v-model="form.username" placeholder="请选择活动区域">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="管理员" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="品牌" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="型号" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="所属/承租公司" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="使用公司" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="使用状况" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="所在位置" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="预计使用期限" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="金额" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="购置/起租日期" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="购置方式" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="订单号" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="计量单位" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                    <el-form-item label="备注" prop="">
                        <el-input v-model="form.username"/>
                    </el-form-item>
                </div>
            </div>

            <!--<div class="footer-btn">-->
                <!--<el-form-item>-->
                    <!--<el-button type="primary" @click="">立即创建</el-button>-->
                    <!--<el-button>取消</el-button>-->
                <!--</el-form-item>-->
            <!--</div>-->

        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button :size="common.btnSize" :loading="loading" type="primary" @click="doSubmit">确认</el-button>
            <el-button :size="common.btnSize" @click="cancel">取消</el-button>
        </div>
    </el-dialog>
</template>

<script>

  import { add, edit } from '@/api/user'
  import { getDepts } from '@/api/dept'
  import { getAll, getLevel } from '@/api/role'
  import { getAllJob } from '@/api/job'
  import Treeselect from '@riophae/vue-treeselect'
  import '@riophae/vue-treeselect/dist/vue-treeselect.css'
  export default {
    components: { Treeselect },
    props: {
      isAdd: {
        type: Boolean,
        required: true
      }
    },
    data() {
      const validPhone = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请输入电话号码'))
        } else if (!this.isvalidPhone(value)) {
          callback(new Error('请输入正确的11位手机号码'))
        } else {
          callback()
        }
      }
      return {
        dialog: false,
        loading: false,
        options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        form: {
          username: '',
          email: '',
          enabled: 'false',
          roles: [],
          job: { id: '' },
          dept: { id: '' },
          phone: null
        },
        roleIds: [], roles: [], depts: [], deptId: null, jobId: null, jobs: [], style: 'width: 184px', level: 3,
        rules: {
          username: [
            { required: true, message: '请输入用户名', trigger: 'blur' },
            { min: 2, max: 20, message: '长度在 2 到 20 个字符', trigger: 'blur' }
          ],
          email: [
            { required: true, message: '请输入邮箱地址', trigger: 'blur' },
            { type: 'email', message: '请输入正确的邮箱地址', trigger: 'blur' }
          ],
          phone: [
            { required: true, trigger: 'blur', validator: validPhone }
          ],
          enabled: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created() {
      const explorer = navigator.userAgent
      if (explorer.indexOf('Chrome') >= 0) {
        this.style = 'width: 184px'
      } else {
        this.style = 'width: 172px'
      }
    },
    methods: {
      cancel() {
        this.resetForm()
      },
      doSubmit() {
        this.form.dept.id = this.deptId
        this.form.job.id = this.jobId
        this.$refs['form'].validate((valid) => {
          if (valid) {
            if (this.deptId === null || this.deptId === undefined) {
              this.$message({
                message: '部门不能为空',
                type: 'warning'
              })
            } else if (this.jobId === null) {
              this.$message({
                message: '岗位不能为空',
                type: 'warning'
              })
            } else if (this.roleIds.length === 0) {
              this.$message({
                message: '角色不能为空',
                type: 'warning'
              })
            } else {
              this.loading = true
              this.form.roles = []
              const _this = this
              this.roleIds.forEach(function(data, index) {
                const role = { id: data }
                _this.form.roles.push(role)
              })
              if (this.isAdd) {
                this.doAdd()
              } else this.doEdit()
            }
          } else {
            return false
          }
        })
      },
      doAdd() {
        add(this.form).then(res => {
          this.resetForm()
          this.$notify({
            title: '添加成功',
            message: '默认密码：123456',
            type: 'success',
            duration: 2500
          })
          this.loading = false
          this.$parent.init()
        }).catch(err => {
          this.loading = false
          console.log(err.response.data.message)
        })
      },
      doEdit() {
        edit(this.form).then(res => {
          this.resetForm()
          this.$notify({
            title: '修改成功',
            type: 'success',
            duration: 2500
          })
          this.loading = false
          this.$parent.init()
        }).catch(err => {
          this.loading = false
          console.log(err.response.data.message)
        })
      },
      resetForm() {
        this.dialog = false
        this.$refs['form'].resetFields()
        this.deptId = null
        this.jobId = null
        this.roleIds = []
        this.form = { username: '', email: '', enabled: 'false', roles: [], job: { id: '' }, dept: { id: '' }, phone: null }
      },
      getRoles() {
        getAll().then(res => {
          this.roles = res
        }).catch(err => {
          console.log(err.response.data.message)
        })
      },
      getJobs(id) {
        getAllJob(id).then(res => {
          this.jobs = res.content
        }).catch(err => {
          console.log(err.response.data.message)
        })
      },
      getDepts() {
        getDepts({ enabled: true }).then(res => {
          this.depts = res.content
        })
      },
      isvalidPhone(str) {
        const reg = /^1[3|4|5|7|8][0-9]\d{8}$/
        return reg.test(str)
      },
      selectFun(node, instanceId) {
        this.getJobs(node.id)
      },
      getRoleLevel() {
        getLevel().then(res => {
          this.level = res.level
        }).catch(err => {
          console.log(err.response.data.message)
        })
      }
    }
  }
</script>

<style scoped>

</style>
