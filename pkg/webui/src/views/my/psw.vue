<template>
  <div>
    <el-form :model="form">
      <el-form-item label-width="100px" label="新密码">
        <el-input v-model="form.re_password" type="password" placeholder="请输入" autocomplete="off" @blur="handleBlur($event)"/>
      </el-form-item>
      <el-form-item label-width="100px" label="确认新密码">
        <el-input v-model="form.new_password" type="password" placeholder="请输入" autocomplete="off" @blur="handleBlur($event)"/>
      </el-form-item>
      <el-form-item label-width="100px">
        <el-button type="primary" @click="toUpWord">确 定</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
import { updateUserPassWord } from '@/api/user'
export default {
  data() {
    return {
      form: {
        re_password: '',
        new_password: ''
      }
    }
  },
  methods: {
    handleBlur(event) {
      if (event.target.value.length < 6) {
        this.$message.error('密码长度不能少于 6 位')
        event.target.value = ''
      }
    },
    toUpWord() {
      if (this.form.re_password === '') {
        this.$message.error('请输入新密码')
        return
      }
      if (this.form.new_password === '') {
        this.$message.error('请输入确认新密码')
        return
      }
      if (this.form.re_password.length < 6 || this.form.new_password.length < 6) {
        this.$message.error('密码长度不能少于 6 位')
        return
      }
      var patt1 = new RegExp(/\s+/g)
      if (patt1.test(this.form.re_password) || patt1.test(this.form.new_password)) {
        this.$message.error('密码仅支持输入字母，数字，符号')
        return
      }
      updateUserPassWord(this.form).then(() => {
        // this.$message.success('密码修改成功')
        this.$confirm('登录已失效, 请重新登录！', '密码修改成功', {
          confirmButtonText: '确定',
          showCancelButton: false,
          type: 'success'
        }).then(() => {
          // this.logout()
          this.$store.dispatch('LogOut').then(() => {
            location.reload()// In order to re-instantiate the vue-router object to avoid bugs
          })
        }).catch(() => {
        })
      }).catch((res) => {
        // this.$message.error(res.msg)
      })
    }
  }
}
</script>
