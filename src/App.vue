<template>
  <div :style="appVars">
    <transition name="banner-fade">
      <div v-if="announcement.visible" class="top-banner" role="region" aria-label="announcement">
        <div class="top-banner__inner">
          <div class="top-banner__left">
            <span class="top-banner__badge">活动</span>
            <span class="top-banner__text">{{ announcement.content }}</span>
          </div>
          <div class="top-banner__right">
            <el-button
              v-if="announcement.link"
              type="primary"
              size="small"
              class="top-banner__cta"
              @click="openAnnouncementLink"
            >
              查看详情
            </el-button>
            <button
              type="button"
              class="top-banner__close"
              aria-label="close"
              @click="closeAnnouncement"
            >
              ×
            </button>
          </div>
        </div>
      </div>
    </transition>
    <HeaderNav v-if="state.isShowNav"></HeaderNav>
    <router-view :style="contentStyle"></router-view>
  </div>
</template>

<script>
import HeaderNav from "@/components/HeaderNav.vue";
import { computed, onMounted, reactive } from "vue";
import { useRoute } from 'vue-router';
import request from "@/utils/request";
export default {
  name: 'App',
  components: {
    HeaderNav
  },
  watch: {
    $route: function (val, oldVal) {
      this.routeChange(val, oldVal);
    },
  },
  setup() {
    const state = reactive({
      isShowNav: false,
      isShowSlider: false,
    });

    const announcement = reactive({
      visible: false,
      content: "",
      link: ""
    });

    const route = useRoute();
    const routeChange = (val,oldVal) => {
      state.isShowNav = val.path !== "/";
      const hiddenNavs = [
        "/login","/register"
      ];
      state.isShowNav = !hiddenNavs.includes(val.path);
    };

    const fetchAnnouncement = () => {
      request.get("/api/announcement").then(res => {
        const data = res.data || {}
        const enabled = Number(data.enabled || 0)
        const content = String(data.content || "")
        const link = String(data.link || "")
        const closedKey = `announcement_closed_${content}`
        const closed = sessionStorage.getItem(closedKey) === "1"
        announcement.visible = enabled === 1 && content.length > 0 && !closed
        announcement.content = content
        announcement.link = link
      })
    }

    const closeAnnouncement = () => {
      const closedKey = `announcement_closed_${announcement.content}`
      sessionStorage.setItem(closedKey, "1")
      announcement.visible = false
    }

    const openAnnouncementLink = () => {
      if (!announcement.link) return
      window.open(announcement.link, "_blank", "noopener")
    }

    const appVars = computed(() => {
      return {
        "--banner-offset": announcement.visible ? "44px" : "0px"
      }
    })

    const contentStyle = computed(() => {
      const headerHeight = state.isShowNav ? 60 : 0
      const bannerHeight = announcement.visible ? 44 : 0
      return {
        paddingTop: `calc(${headerHeight + bannerHeight}px + 0.5rem)`
      }
    })

    onMounted(() => {
      routeChange(route, route);
      fetchAnnouncement()
    })
    return {
      state,
      announcement,
      contentStyle,
      appVars,
      closeAnnouncement,
      openAnnouncementLink,
      routeChange,
    };
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
*{
  margin: 0;
  padding: 0;
}
html{
  background-color: rgb(18,25,38);
}
html,body{
  height: 100%;
}

.banner-fade-enter-active,
.banner-fade-leave-active {
  transition: opacity 0.18s ease, transform 0.18s ease;
}

.banner-fade-enter-from,
.banner-fade-leave-to {
  opacity: 0;
  transform: translateY(-6px);
}

.top-banner {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 2000;
  height: 44px;
  display: flex;
  align-items: center;
  padding: 0 12px;
  background: linear-gradient(90deg, rgba(64, 158, 255, 0.22), rgba(128, 90, 213, 0.20), rgba(0, 210, 255, 0.18));
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}

.top-banner__inner {
  width: 100%;
  max-width: 1180px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}

.top-banner__left {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
}

.top-banner__badge {
  display: inline-flex;
  align-items: center;
  height: 22px;
  padding: 0 10px;
  border-radius: 999px;
  font-size: 12px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.92);
  background: rgba(0, 129, 255, 0.22);
  border: 1px solid rgba(0, 129, 255, 0.35);
  flex: 0 0 auto;
}

.top-banner__text {
  color: rgba(255, 255, 255, 0.92);
  font-size: 13px;
  line-height: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.top-banner__right {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  flex: 0 0 auto;
}

.top-banner__cta {
  border: 0;
  background: linear-gradient(90deg, rgba(64, 158, 255, 0.95), rgba(108, 99, 255, 0.95));
}

.top-banner__close {
  width: 28px;
  height: 28px;
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  background: rgba(0, 0, 0, 0.18);
  color: rgba(255, 255, 255, 0.9);
  font-size: 18px;
  line-height: 26px;
  cursor: pointer;
}

.top-banner__close:hover {
  background: rgba(0, 0, 0, 0.28);
}

@media (max-width: 768px) {
  .top-banner__inner {
    gap: 8px;
  }
  .top-banner__cta {
    padding: 0 10px;
  }
  .top-banner__badge {
    display: none;
  }
}
</style>
