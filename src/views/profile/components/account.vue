<template>
  <el-form ref="form_account" :model="form"  class="form" label-position="top" :rules="rules">
    <el-form-item label="用户名" prop="username">
      <el-input v-model="form.username"  ></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submit">提交</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import {mapGetters, useStore} from "vuex";
import { reactive, ref } from "vue";
import request from "../../../utils/request";
import {ElMessage} from "element-plus";
import store from "../../../store";

export default {
  name: "account",
  setup(){
    const store = useStore();
    const user = store.getters.user
    const form = ref({
      username: user.username
    })
    return {
      form,
      rules: {
        username: [
          { required: true, message: '请输入活动名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ]
      },
    }
  },
  computed: {
    ...mapGetters([
      'user',
    ])
  },
  methods:{
    submit(){
      this.$refs['form_account'].validate((valid) => {
        if(valid){
          let data = {
            username:this.form.username
          }
          request.post('/api/user',data).then(res=>{
            ElMessage({
              message:"信息更新成功",
              type: "success"
            })
            store.dispatch('getInfo')
          })
        }else{
          console.log("valid error")
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

  .el-input-group__prepend {
    border: none;
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