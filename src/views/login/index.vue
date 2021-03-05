<template>
    <div class="login">
        <el-form ref="form" :model="form" :rules="rules" label-width="80px">
            <el-form-item label="手机号" prop="phone">
                <el-input v-model="form.phone"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
                <el-input v-model="form.password"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button
                    type="primary"
                    @click="onSubmit"
                    class="login-btn"
                    :loading="isLoginLoading"
                >
                    登录
                </el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script lang="ts">
import Vue from 'vue'
import { Form } from 'element-ui'
import { login } from '../../utils/user'
import { mapMutations } from 'vuex'

export default Vue.extend({
  name: 'LoginIndex',
  data () {
    return {
      form: {
        phone: '13811473704',
        password: '123456'
      },
      rules: {
        phone: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          { pattern: /^1\d{10}$/, message: '请输入正确的手机号' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 18, message: '长度在6到18个字符', trigger: 'blur' }
        ]
      },
      isLoginLoading: false
    }
  },
  methods: {
    ...mapMutations(['setUser']),
    async onSubmit () {
      try {
        // 1:表单验证
        await (this.$refs.form as Form).validate()
        this.isLoginLoading = true
        // 2:表单提交
        const { data } = await login(this.form)
        if (data.state !== 1) {
          return this.$message.error(data.message)
        }
        this.setUser(data.content)
        this.$router.push(this.$route.query.redirect as string || '/')
        this.$message.success('登录成功')
      } catch (err) {
        this.$message.error('登录失败')
      }
      this.isLoginLoading = false
    }
  }
})
</script>
<style lang="scss" scoped>
.login {
    height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    .login-form {
        width:300px;
        background:#fff;
        padding:20px;
        border-radius:5px;
    }
    .login-btn{
        width:100%;
    }
}
</style>
