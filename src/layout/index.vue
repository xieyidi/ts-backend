<template>
    <div>
        <el-container>
            <el-side width="200px">
                <Aside />
            </el-side>
            <el-container>
                <el-header>
                    <layout-header />
                </el-header>
                <el-main>
                    Main
                    <router-view />
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>
<script lang="ts">
import Vue from 'vue'
import Aside from './component/aside.vue'
import LayoutHeader from './component/layout-header.vue'
export default Vue.extend({
  name: 'Layout',
  components: {
    Aside,
    LayoutHeader
  }
})
</script>
<style lang="scss" scoped>
.el-container{
    min-height:100vh;
    // min-width:980px;
}
.el-aside{
    background:#d3dce6;
}
.el-header{
    background:#b3c0d1;
}
.el-main{
    background:#e9eef3
}
</style>
