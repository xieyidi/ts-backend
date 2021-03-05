<template>
    <div class="header">
        <el-breadcrumb
            separator-class="el-icon-arrow-right"
        >
            <el-breadcrumb-item
                :to="{path:'/'}"
            >
                首页
            </el-breadcrumb-item>
            <el-breadcrumb-item>活动管理</el-breadcrumb-item>
            <el-breadcrumb-item>活动列表</el-breadcrumb-item>
            <el-breadcrumb-item>活动详情</el-breadcrumb-item>
        </el-breadcrumb>
        <el-dropdown>
            <span class="el-dropdown-link">
                <el-avatar
                    id="header-avatar"
                    shape="sqaure"
                    :size="30"
                    :src="userInfo.portrait || require('@/assets/avatar.jpeg')"
                >
                </el-avatar>
                <i class="el-icon-arrow-down el-icon--right"></i>
            </span>
            <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>{{ userInfo.userName }}</el-dropdown-item>
                <el-dropdown-item @click="handleLogout" divided>退出</el-dropdown-item>
            </el-dropdown-menu>
        </el-dropdown>
    </div>
</template>
<script lang="ts">
import Vue from 'vue'
import { mapMutations } from 'vuex'
import { getUserInfo } from '@/utils/user'
export default Vue.extend({
  name: 'AppHeader',
  data () {
    return {
      userInfo: {}
    }
  },
  created () {
    this.loadUserInfo()
  },
  methods: {
    ...mapMutations(['setUser']),
    async loadUserInfo () {
      const { data } = await getUserInfo()
      this.userInfo = data.content
    },
    async handleLogout () {
      this.$confirm('确认退出吗？', '退出提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.setUser(null)
        this.$router.push({
          name: 'login'
        })
        this.$message({
          type: 'success',
          message: '退出成功'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消'
        })
      })
    }
  }
})
</script>
<style lang="scss">
.header{
    display:flex;
    align-items:center;
    height:100%;
    justify-content:space-between;
    .el-dropdown-link{
        display:flex;
        align-items:center;
        #header-avatar {
            img{
                width:100%;
                height:auto;
            }
        }
    }
}
</style>
