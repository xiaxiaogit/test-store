<template>
  <div class="login">
    <div class="box">
      <h3 class="center">登录</h3>
      <div class="line">
        <el-input
          placeholder="请输入账号"
          clearable
          v-model="form.username"
        ></el-input>
      </div>
      <div class="line">
        <el-input
          placeholder="请输入密码"
          clearable
          v-model="form.password"
          show-password
        ></el-input>
      </div>
      <div class="center line">
        <el-button type="primary" @click="login">登录</el-button>
      </div>
    </div>
  </div>
</template>
<script>
import { mapGetters, mapActions } from "vuex";
import { reqLogin } from "../../utils/request";
import { successAlert, warningAlert } from "../../utils/alert";

export default {
  props: [],
  components: {},
  data() {
    return {
      form: {
        username: "",
        password: ""
      }
    };
  },
  computed: {
    ...mapGetters({})
  },
  methods: {
    ...mapActions({
      changeUserAction: "changeUserAction"
    }),
    login() {
      reqLogin(this.form).then(res => {
        if (res.data.code == 200) {
          successAlert(res.data.msg);
          // res.data.list  用户信息 存起来，供很多组件使用
          this.changeUserAction(res.data.list)
          this.$router.push("/")
        } else {
          warningAlert(res.data.msg);
        }
      });
    }
  },
  mounted() {}
};
</script>
<style scoped>
.login {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(to right, #553443, #303d60);
  position: relative;
}
.box {
  width: 400px;
  padding: 20px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: #ffffff;
  border-radius: 20px;
}
.center {
  text-align: center;
}
.line {
  margin-top: 20px;
}
</style>
