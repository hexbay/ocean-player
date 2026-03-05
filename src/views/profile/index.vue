<template>
  <div class="container">
    <div class="profile-page">
      <el-row :gutter="20">
        <el-col :span="6" :xs="24">
          <el-card class="glass-card profile-card">
            <div class="profile-card__header">
              <div class="profile-card__title">个人信息</div>
              <el-avatar v-if="user.avatar" :src="user.avatar" class="profile-card__avatar" />
              <el-avatar v-else class="profile-card__avatar">
                {{ user.username.slice(0, 1).toUpperCase() }}
              </el-avatar>
              <div class="profile-card__name">{{ user.username }}</div>
            </div>
          </el-card>
        </el-col>
        <el-col :span="18" :xs="24">
          <el-card class="glass-card profile-card">
            <el-tabs v-model="activeTab" tab-position="left">
              <el-tab-pane label="账户信息" name="account">
                <account  />
              </el-tab-pane>
              <el-tab-pane label="安全" name="sec">
                <rest-pass :user="user" />
              </el-tab-pane>
            </el-tabs>
          </el-card>
        </el-col>
      </el-row>

    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Account from './components/account'
import RestPass from './components/restPass'
import { ref } from "vue";
export default {
  name: 'Profile',
  components: {Account,RestPass},
  setup() {
    const activeTab= ref("account")
    return {
      activeTab
    }
  },
  computed: {
    ...mapGetters([
      'user',
    ])
  },
  created() {
    this.getUser()
  },
  methods: {
    getUser() {

    }
  }
}
</script>
<style lang="scss" scoped>
.profile-page {
  margin-top: 16px;
}

:deep(.glass-card.el-card) {
  color: rgba(255, 255, 255, 0.92);
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.06) 0%,
    rgba(17, 25, 40, 0.58) 30%,
    rgba(17, 25, 40, 0.40) 100%
  ) !important;
  border: 1px solid rgba(148, 163, 184, 0.18) !important;
  border-radius: 16px;
  box-shadow:
    0 18px 45px rgba(0, 0, 0, 0.18),
    inset 0 1px 0 rgba(255, 255, 255, 0.10);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
}

:deep(.glass-card.el-card .el-card__body) {
  padding: 18px;
}

.profile-card__header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.profile-card__title {
  font-weight: 700;
  letter-spacing: 0.02em;
  color: rgba(255, 255, 255, 0.86);
}

.profile-card__avatar {
  width: 72px;
  height: 72px;
  box-shadow:
    0 12px 30px rgba(0, 0, 0, 0.28),
    0 0 0 3px rgba(64, 158, 255, 0.12);
}

.profile-card__name {
  font-size: 16px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.92);
}

:deep(.el-tabs) {
  color: rgba(255, 255, 255, 0.92);
}

:deep(.el-tabs__header) {
  margin: 0 18px 0 0;
}

:deep(.el-tabs__nav-wrap::after) {
  background-color: rgba(148, 163, 184, 0.14);
}

:deep(.el-tabs__item) {
  color: rgba(255, 255, 255, 0.78);
  border-radius: 10px;
  margin: 6px 0;
  padding: 10px 14px;
  transition: background 0.18s ease, color 0.18s ease;
}

:deep(.el-tabs__item:hover) {
  color: rgba(255, 255, 255, 0.92);
  background: rgba(255, 255, 255, 0.05);
}

:deep(.el-tabs__item.is-active) {
  color: rgba(255, 255, 255, 0.95) !important;
  background: linear-gradient(90deg, rgba(64, 158, 255, 0.22), rgba(139, 92, 246, 0.14));
  box-shadow: 0 10px 22px rgba(0, 0, 0, 0.18);
}

:deep(.el-tabs__active-bar) {
  background: transparent;
}
</style>