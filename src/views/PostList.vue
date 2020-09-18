<template>
<div>
  <!-- data: 列表的数据对象 -->
  <el-table
    :data="tableData"
    style="width: 100%; margin-bottom:20px;">

    <!-- 表格的列 -->
    <!-- label：标签 -->
    <el-table-column
      label="序号"
      width="60">

      <!-- scope.row：数组中当前列的数据对象 -->
      <template slot-scope="scope">
        <span>{{scope.$index + 1}}</span>
      </template>
    </el-table-column>

    <el-table-column
      label="标题"
      width="300">
      <template slot-scope="scope">
        <span>{{scope.row.title}}</span>
      </template>
    </el-table-column>

    <el-table-column
      label="显示"
      width="180">
      <template slot-scope="scope">
        <span>{{scope.row.open === 1 ? '打开': '关闭'}}</span>
      </template>
    </el-table-column>

    <el-table-column
      label="类型"
      width="180">
      <template slot-scope="scope">
        <span>{{scope.row.type === 1 ? '文章': '视频'}}</span>
      </template>
    </el-table-column>

    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">编辑</el-button>

        <!-- 编辑open这个字段 -->
        <el-button
          size="mini"
          :type="danger"
          @click="handleDelete(scope.$index, scope.row)">
          关闭
          </el-button>
      </template>
    </el-table-column>
  </el-table>

   <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pageIndex"
      :page-sizes="[5, 10, 15]"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="100">
    </el-pagination>
</div>
</template>

<script>
  export default {
    data() {
      return {
        tableData: [],
        pageIndex: 1,
        pageSize: 5,
        total: 0
      }
    },
    methods: {
      // index是索引， row是对象
      handleEdit(index, row) {
        console.log(index, row);
      },

      // 关闭或者打开文章
      handleDelete(index, row) {
        console.log(index, row);
      },

      //条数切换时触发
      handleSizeChange(val){
         console.log(`每页 ${val} 条`);
      },

      //切换页数的时候触发
       handleCurrentChange(val) {
        this.pageIndex=val;
        this.getList()
      },

      // 请求文章列表的数据
      getList(){
        // 请求文章列表
        this.$axios({
          url: `/post?pageIndex=${this.pageIndex}&pageSize=${this.pageSize}`
        }).then(res => {
          const {data} = res.data;

          this.tableData = data;
        })
      },
    },
    mounted(){
          this.getList()
    }
  }
</script>