<template>
  <div class="account-wrap">
    <!-- 条件筛选区 -->
    <el-card class="filter-bar">
      <el-row type="flex" align="bottom" :gutter="20">
        <el-col :span="18">
          <el-form label-position="top" label-width="80px" :model="formData" size="small">
            <el-row :gutter="20">
              <el-col :span="8">
                <el-form-item label="英雄">
                  <el-input v-model="formData.name" placeholder="请输入英雄名称" clearable></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="类型">
                  <el-select v-model="formData.type" placeholder="请选择类型" style="width:100%" clearable>
                    <el-option label="射手" value="射手"></el-option>
                    <el-option label="法师" value="法师"></el-option>
                    <el-option label="辅助" value="辅助"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="住址">
                  <el-input v-model="formData.address" placeholder="请输入住址" clearable></el-input>
                </el-form-item>
              </el-col>
            </el-row>
          </el-form>
        </el-col>
        <el-col :span="6">
          <el-button @click="resetForm" size="small">重置</el-button>
          <el-button type="primary" size="small" icon="el-icon-search" @click="getData">搜索</el-button>
          <el-button type="primary" size="small" icon="el-icon-plus" style="float:right">新建</el-button>
        </el-col>
      </el-row>
    </el-card>
    <!-- 表格区 -->
    <el-card>
      <el-table :data="tableData" stripe size="small">
        <el-table-column prop="id" label="#" width="100" align="center"></el-table-column>
        <el-table-column prop="hero" label="英雄" width="120"></el-table-column>
        <el-table-column prop="type" label="类型" width="120"></el-table-column>
        <el-table-column prop="date" label="上架时间" width="150"></el-table-column>
        <el-table-column prop="province" label="省份" width="150"></el-table-column>
        <el-table-column prop="city" label="城市" width="150"></el-table-column>
        <el-table-column prop="address" label="住址"></el-table-column>
        <el-table-column prop="type" label="操作" width="150" align="center">
          <template slot-scope="scope">
            <i title="编辑" class="btn el-icon-edit"></i>
            <i title="复制" class="btn el-icon-copy-document" v-copy="scope.row.address"></i>
            <i title="删除" class="btn el-icon-delete" @click="handleDelete(scope.row)"></i>
          </template>
        </el-table-column>
      </el-table>
      <!-- 分页 -->
      <div class="pagination-wrap">
        <el-pagination
          background
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          layout="total, sizes, prev, pager, next, jumper"
          :total="paginationData.total"
        ></el-pagination>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "account",
  components: {},
  data() {
    return {
      paginationData: {
        total: 0,
        currentPage: 1,
        pageSize: 10,
      },
      formData: {
        name: "",
        type: "",
        address: "",
      },
      tableData: [],
    };
  },
  mounted() {
    this.getData();
  },
  methods: {
    async getData() {
      // let res = await this.$http.get("getTableData", {
      //   params: {
      //     currentPage: this.paginationData.currentPage,
      //     pageSize: this.paginationData.pageSize,
      //   },
      // });
      // this.tableData = res.data.list;
      // this.paginationData.total = res.data.total;
      this.tableData = Array.from({ length: 10 }, () => {
        return this.$mock.mock({
          id: "@increment",
          hero: "@pick(['甄姬','后裔','蔡文姬'])",
          date: "@date",
          province: "@province",
          city: "@city",
          address: "@csentence",
          type: "@pick(['法师','射手','辅助'])",
        });
      });
      this.paginationData.total = 10;
    },
    resetForm() {
      this.formData = {
        name: "",
        type: "",
        address: "",
      };
    },
    handleCurrentChange(num) {
      this.paginationData.currentPage = num;
      this.getData();
    },
    handleSizeChange(num) {
      this.paginationData.pageSize = num;
      this.getData();
    },
    handleDelete(obj) {
      this.$confirm(`确定要删除 ${obj.hero} ?`, "提示")
        .then(() => {
          this.$http.delete(`list/${obj.id}`).then((res) => {
            this.$message({
              type: "success",
              message: "Success!",
            });
            this.getData();
          });
        })
        .catch(() => {});
    },
  },
};
</script>

<style lang="scss" scoped>
.filter-bar::v-deep {
  margin-bottom: 20px;
  .el-card__body {
    padding: 10px 20px 20px 20px;
  }
}
.el-form-item {
  margin-bottom: 0;
}
.btn {
  display: inline-block;
  margin-right: 15px;
  font-size: 18px;
  &:hover {
    font-weight: bold;
    cursor: pointer;
  }
}
.btn.el-icon-edit {
  color: #409eff;
}
.btn.el-icon-copy-document {
  color: #67c23a;
}

.btn.el-icon-delete {
  color: #f56c6c;
}

.pagination-wrap {
  padding: 20px 0 0 0;
  text-align: right;
}
</style>


