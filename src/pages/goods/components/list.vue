<template>
  <div>
    <el-table
      :data="list"
      border
      row-key="id"
      style="width: 100%"
      :tree-props="{ children: 'children' }"
    >
      <el-table-column prop="id" label="商品编号" width="180">
      </el-table-column>

      <el-table-column prop="goodsname" label="商品名称" width="180">
      </el-table-column>

      <el-table-column label="商品价格">
        <template slot-scope="scope">
          <span>{{ scope.row.price | filterPrice }}</span>
        </template>
      </el-table-column>

      <el-table-column label="市场价格">
        <template slot-scope="scope">
          <span>{{ scope.row.market_price | filterPrice }}</span>
        </template>
      </el-table-column>

      <el-table-column label="图片">
        <template slot-scope="scope">
          <img :src="$imgPre + scope.row.img" alt="" class="img" />
        </template>
      </el-table-column>

      <el-table-column label="是否新品">
        <template slot-scope="scope">
          <el-button v-if="scope.row.status == 1" type="primary">是</el-button>
          <el-button v-else type="danger">否</el-button>
        </template>
      </el-table-column>

      <el-table-column prop="name" label="是否热卖">
        <template slot-scope="scope">
          <el-button v-if="scope.row.status == 1" type="primary">是</el-button>
          <el-button v-else type="danger">否</el-button>
        </template>
      </el-table-column>

      <el-table-column prop="name" label="状态">
        <template slot-scope="scope">
          <el-button v-if="scope.row.status == 1" type="primary"
            >启用</el-button
          >
          <el-button v-else type="info">禁用</el-button>
        </template>
      </el-table-column>

      <el-table-column prop="status" label="操作">
        <template slot-scope="scope">
          <el-button type="primary" @click="edit(scope.row.id)">编辑</el-button>
          <del-btn @confirm="dele(scope.row.id)"></del-btn>
        </template>
      </el-table-column>
    </el-table>

    <el-pagination
      background
      layout="prev, pager, next"
      :total="total"
      :page-size="size"
      @current-change="changePage"
    >
    </el-pagination>
  </div>
</template>
<script>
import { mapGetters, mapActions } from "vuex";
import { successAlert, warningAlert } from "../../../utils/alert";
import { reqGoodsDel } from "../../../utils/request";
export default {
  props: [],
  components: {},
  data() {
    return {};
  },
  computed: {
    ...mapGetters({
      list: "goods/list",
      size: "goods/size",
      total: "goods/total"
    })
  },
  methods: {
    ...mapActions({
      reqListAction: "goods/reqListAction",
      reqTotalAction: "goods/reqTotalAction",
      changePageAction: "goods/changePageAction"
    }),
    //编辑
    edit(id) {
      this.$emit("edit",id);
    },
    //删除2
    dele(id) {
      //点了确定按钮
      reqGoodsDel(id).then(res => {
        if (res.data.code == 200) {
          successAlert(res.data.msg);
          this.reqListAction();
          //重新获取总数
          this.reqTotalAction();
        } else {
          warningAlert(res.data.msg);
        }
      });
    },
    //修改了当前页码
    changePage(e) {
      this.changePageAction(e);
    }
  },
  mounted() {
    //获取的list
    this.reqListAction();
    //获取list的总数
    this.reqTotalAction();
  }
};
</script>
<style scoped>
.img {
  width: 100px;
  height: 100px;
}
</style>
