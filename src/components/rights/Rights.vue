<template>
  <div>
    <!-- 面包屑导航区 -->
    <el-breadcrumb separator="/">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>权限列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图  -->
    <el-card>
      <el-table :data="rightList" border stripe>
        <el-table-column type="index"></el-table-column>
        <el-table-column label="权限名称" prop="authName"></el-table-column>
        <el-table-column label="路径" prop="path"></el-table-column>
        <el-table-column label="权限等级" prop="level">
          <!-- 通过作用域插槽自定义输出格式 scope接受数据-->
          <template slot-scope="scope">
            <el-tag type="success" v-if="scope.row.level === '0'">一级权限</el-tag>
            <el-tag type="warning" v-else-if="scope.row.level === '1'">二级权限</el-tag>
            <el-tag type="danger" v-else>三级权限</el-tag>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rightList: [], //权限列表
    };
  },
  created() {
    this.getRightsList();
  },
  methods: {
    //获取权限列表
    async getRightsList() {
      const { data: res } = await this.$http.get("rights/list");
      if (res.meta.status !== 200) {
        return this.$message.error("获取列表失败");
      }
      this.rightList = res.data;
      // console.log(this.rightList);
    },
  },
};
</script>

<style lang="less" scoped>
</style>