<template>
  <el-form :model="form" ref="form_rest"  class="form" label-position="top" :rules="rules">
    <el-form-item label="当前密码" prop="old_password">
      <el-input  v-model="form.old_password" :show-password="true"></el-input>
    </el-form-item>
    <el-form-item label="密 码" prop="password">
      <el-input  v-model="form.password" :show-password="true"></el-input>
    </el-form-item>
    <el-form-item label="确认密码" prop="confirm_password">
      <el-input  v-model="form.confirm_password" :show-password="true"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary"  @click="submit">提交</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import {mapGetters, useStore} from "vuex";
import { ref } from "vue";
import request from "../../../utils/request";
import {ElMessage} from "element-plus";
import store from "../../../store";

export default {
  name: "rest-pass",
  setup() {
    const store = useStore();
    const user = store.getters.user
    const form = ref({
      old_password: null,
      confirm_password: null,
      password: null
    })
    const validatePass2 = (rule, value, callback) => {
      console.log(value,form,form.value.password)
      if (form.value.confirm_password === form.value.password) {
        callback();
      } else {
        callback(new Error("两次输入密码不一致!"));
      }
    };
    return {
      form,
      rules: {
        old_password: [
          { required: true, message: '请属于原密码', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ],
        confirm_password: [
          { required: true, message: '请再次确认密码', trigger: 'blur' },
          { validator: validatePass2, trigger: "blur" }
        ]
      }
    }
  },
  computed: {
    ...mapGetters([
      'user',
    ])
  },
  methods: {
    submit() {
      this.$refs['form_rest'].validate((valid) => {
        if(valid){
          let data = {
            password: this.form.password,
            old_password: this.form.old_password
          }
          request.post('/api/rest_pass', data).then(res => {
            ElMessage({
              message: "信息更新成功",
              type: "success"
            })
          }).catch()
        }else{
          return false
        }
      })

    }
  }

}
</script>
<style lang="scss" scoped>
.form {
  color: rgba(255, 255, 255, 0.92);

  :deep(.el-form-item__label) {
    color: rgba(255, 255, 255, 0.72);
    font-weight: 600;
  }

  :deep(.el-input__wrapper) {
    background: linear-gradient(
      180deg,
      rgba(17, 25, 40, 0.55) 0%,
      rgba(17, 25, 40, 0.35) 100%
    );
    border: 1px solid rgba(148, 163, 184, 0.18);
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.16);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
  }

  :deep(.el-input__wrapper.is-focus) {
    border-color: rgba(64, 158, 255, 0.55);
    box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.14);
  }

  :deep(.el-input__inner) {
    color: rgba(255, 255, 255, 0.92);
  }

  :deep(.el-button--primary) {
    border: 0;
    background: linear-gradient(90deg, rgba(64, 158, 255, 0.95), rgba(108, 99, 255, 0.95));
    box-shadow: 0 16px 32px rgba(0, 0, 0, 0.22);
  }
}
</style>