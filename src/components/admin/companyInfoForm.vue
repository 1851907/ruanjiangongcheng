<template>
  <div>
    <h3>公司信息</h3>
    <el-form class="col-height" :model="this.detail" label-width="150px">
      <el-form-item label="公司名称:">
        <el-input
          v-model="this.detail.companyName"
          readonly
          style="width: 82%"
        ></el-input>
      </el-form-item>
      <el-form-item label="地址:">
        <el-input
          v-model="this.detail.address"
          readonly
          style="width: 82%"
        ></el-input>
      </el-form-item>
      <el-form-item label="资产报告:">
        <quill-view-pop :content="this.detail.asset"></quill-view-pop>
      </el-form-item>
      <el-form-item label="信用等级:">
        <el-popover
          placement="top"
          width="100"
          trigger="click"
          v-model="showRate"
        >
          <div style="margin: auto">
            <el-rate
              v-model="credit_rate"
              show-score
              allow-half
              text-color="#ff9900"
              score-template="{value}"
            >
            </el-rate>
            <!-- 提交按钮 -->
            <el-button
              style="margin: auto"
              @click="rateCompany()"
              icon="el-icon-upload2"
              type="text"
            >
              提交
            </el-button>
          </div>

          <!-- 评分按钮 -->
          <el-button slot="reference" icon="el-icon-s-data" plain>
            修改信用等级</el-button
          >
        </el-popover>
      </el-form-item>
      <el-form-item label="信用积分:">
        <el-input
          v-model="this.detail.creditScore"
          readonly
          style="width: 82%"
        ></el-input>
      </el-form-item>
      <el-form-item label="开户银行:">
        <el-input
          v-model="this.detail.depositBank"
          readonly
          style="width: 82%"
        ></el-input>
      </el-form-item>
      <el-form-item label="开户银行卡号:">
        <el-input
          v-model="this.detail.depositBankCardNumber"
          readonly
          style="width: 82%"
        ></el-input>
      </el-form-item>
      <el-form-item label="公司描述:">
        <quill-view-pop :content="this.detail.description"></quill-view-pop>
      </el-form-item>
      <el-form-item label="电邮地址:">
        <el-input
          v-model="this.detail.email"
          style="width: 82%"
          readonly
        ></el-input>
      </el-form-item>
      <el-form-item label="电话:">
        <el-input
          v-model="this.detail.phone"
          readonly
          style="width: 82%"
        ></el-input>
      </el-form-item>
      <el-form-item label="logo:">
        <el-avatar
          shape="square"
          :size="200"
          fit="cover"
          :src="this.detail.logo"
        ></el-avatar>
      </el-form-item>
      <el-form-item label="在职人数:">
        <el-input
          v-model="this.detail.memberNumber"
          readonly
          style="width: 82%"
        ></el-input>
      </el-form-item>
      <el-form-item label="状态:">
        <el-input v-model="status_name" readonly style="width: 82%"></el-input>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
import quillViewPop from "./quillViewPop.vue";
export default {
  components: { quillViewPop },
  data() {
    return {
      showRate: false,
      credit_rate: 0,
    };
  },
  props: {
    detail: {},
  },
  computed: {
    status_name: function () {
      return this.statusToStr(this.detail.status);
    },
  },
  methods: {
    statusToStr(status_int) {
      switch (status_int) {
        case 0:
          return "待审核";
          break;
        case 1:
          return "已通过";
          break;
        case 2:
          return "未通过";
          break;
        default:
          return "未定义";
          break;
      }
    },
    async rateCompany() {
      let response = await this.$axios.put(this.$api.adminRateCompany, {
        creditRate: this.credit_rate * 20,
        id: this.detail.companyId,
      });
    },
  },
};
</script>

<style scoped>
img {
  width: auto;
  height: auto;
  max-width: 100%;
  max-height: 100%;
}
</style>
