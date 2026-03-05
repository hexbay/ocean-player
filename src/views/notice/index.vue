<template>
  <div class="container">
    <el-timeline style="margin-top: 30px">
      <el-timeline-item v-for="item in list" :timestamp="item.create_time" placement="top" >
        <el-card class="notice-card" style="position: relative">
          <img v-if="item.is_top" src="../../assets/置顶.svg" style="position: absolute;width: 50px;left: 0;top: 0">
          <p>{{ item.content }}</p>
        </el-card>
      </el-timeline-item>
    </el-timeline>
  </div>
</template>

<script>
import request from "../../utils/request";
import { ref } from "vue";

export default {
  name: "index",
  created() {
    this.fetchList()
  },
  setup() {
    const list = ref([])
    return {
      list
    }
  },
  methods: {
    fetchList() {
      request.get('/api/notice').then(res => {
        this.list = res.data;
      })
    }
  }
}
</script>

<style scoped>
.container {
  padding-top: 0;
}

.notice-card {
  color: rgba(255, 255, 255, 0.94);
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.12) 0%,
    rgba(59, 130, 246, 0.10) 20%,
    rgba(17, 25, 40, 0.46) 55%,
    rgba(17, 25, 40, 0.32) 100%
  );
  border: 1px solid rgba(148, 163, 184, 0.22);
  border-radius: 14px;
  box-shadow:
    0 18px 45px rgba(0, 0, 0, 0.16),
    inset 0 1px 0 rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
}

.notice-card:hover {
  border-color: rgba(64, 158, 255, 0.28);
  box-shadow:
    0 22px 60px rgba(0, 0, 0, 0.22),
    0 0 0 1px rgba(64, 158, 255, 0.08);
}

:deep(.el-card__body) {
  text-align: left;
  line-height: 1.6;
  padding: 16px 16px 18px;
}

:deep(.el-timeline) {
  padding-left: 8px;
}

:deep(.el-timeline-item__timestamp) {
  color: rgba(255, 255, 255, 0.68) !important;
  font-size: 12px;
}

:deep(.el-timeline-item__node) {
  background: rgba(64, 158, 255, 0.90) !important;
  box-shadow:
    0 0 0 3px rgba(64, 158, 255, 0.22),
    0 10px 26px rgba(64, 158, 255, 0.10);
}

:deep(.el-timeline-item__tail) {
  border-left-color: rgba(148, 163, 184, 0.26) !important;
}
</style>