<template>
  <a-form style="margin-top: 100px;" :model="form" :style="{width:'600px'}" @submit-success="handleSubmit">
    <a-form-item  :validate-trigger="['input','blur']" field="name" :rules="nameRules" label="手机号">
      <a-input v-model="form.name" placeholder="请输入手机号" />
    </a-form-item>
    <a-form-item :validate-trigger="['input','blur']" field="passWord" :rules="passRules" label="密码">
      <a-input-password v-model="form.passWord" placeholder="请输入密码" />
    </a-form-item>
    <a-form-item field="isRead">
      <a-checkbox v-model="form.isRead">
        记住账号
      </a-checkbox>
    </a-form-item>
    <a-form-item>
      <a-button html-type="submit">登录</a-button>
    </a-form-item>
  </a-form>
  <span class="" style="margin-left: 100px;">
    {{form}}
  </span>
</template>

<script>
import { http, setHttpToken, clearHttpToken } from "../http/http";
export default {
  name: "from",
  data() {
    return {
      form: {
        name: '15727527694',
        passWord: '12345678',
        isRead: false,
      },
      passRules:[{
        required:true,
        message:'请输入密码'
      }],
      nameRules: [{
        required:true,
        message:'请输入用户名/手机号'
      },{
        validator: (value, cb) => {
          const regex = new RegExp(/^(?:(?:\+|00)86)?1[3-9]\d{9}$/);
          return new Promise(resolve => {
              if (!regex.test(value)) {
                cb('请输入正确的手机号')
              }
              resolve()
          })
        }
      }]
    }
  },
  mounted() {
    clearHttpToken();
  },
  methods:{
    async handleSubmit(data) {
      try {
        const response = await http.post(
          "api/user/UserIndex/login",
          this.form
        );
        const { token } = response.data;
        // 存储token
        setHttpToken(token);
        // this.$message.success("登陆成功");
      } catch (error) {

      }
    }
  }
}
</script>

<style scoped>

</style>
