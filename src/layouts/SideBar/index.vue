<template>
  <div class="layout-sidebar-container" :class="{ 'is-collapse': collapse }">
    <Logo />

    <el-scrollbar>
      <el-menu
        background-color="#001529"
        text-color="hsla(0,0%,100%,.65)"
        active-text-color="#fff"
        :defaultActive="activeMenu"
        :collapse="collapse"
        :unique-opened="true"
        router
      >
        <el-menu-item index="/index">
          <el-icon>
            <component is="HomeFilled"></component>
          </el-icon>
          <template #title>
            <span>首页</span>
          </template>
        </el-menu-item>
        <!-- 只有用户昵称为 超级管理员 时才显示系统管理菜单 -->
        <el-sub-menu index="1" v-if="userInfo.name === '超级管理员'">
          <template #title>
            <el-icon>
              <component is="Setting"></component>
            </el-icon>
            <span>系统管理</span>
          </template>
          <el-menu-item index="/system/user">
            <el-icon>
              <component is="UserFilled"></component>
            </el-icon>
            <template #title>
              <span>后台用户管理</span>
            </template>
          </el-menu-item>
          <el-menu-item index="/system/post">
            <el-icon>
              <component is="Postcard"></component>
            </el-icon>
            <template #title>
              <span>岗位管理</span>
            </template>
          </el-menu-item>
        </el-sub-menu>
        <el-sub-menu index="2">
          <template #title>
            <el-icon>
              <component is="OfficeBuilding"></component>
            </el-icon>
            <span>公寓管理</span>
          </template>
          <el-menu-item
            index="/apartmentManagement/apartmentManagement/apartmentManagement"
          >
            <el-icon>
              <component is="OfficeBuilding"></component>
            </el-icon>
            <template #title>
              <span>公寓管理</span>
            </template>
          </el-menu-item>
          <el-menu-item
            index="/apartmentManagement/roomManagement/roomManagement"
          >
            <el-icon>
              <component is="House"></component>
            </el-icon>
            <template #title>
              <span>房间管理</span>
            </template>
          </el-menu-item>
          <el-menu-item
            index="/apartmentManagement/attributeManagement/attributeManagement"
          >
            <el-icon>
              <component is="List"></component>
            </el-icon>
            <template #title>
              <span>属性管理</span>
            </template>
          </el-menu-item>
        </el-sub-menu>
        <el-sub-menu index="3">
          <template #title>
            <el-icon>
              <component is="Management"></component>
            </el-icon>
            <span>租赁管理</span>
          </template>
          <el-menu-item index="/rentManagement/appointment/appointment">
            <el-icon>
              <component is="Clock"></component>
            </el-icon>
            <template #title>
              <span>看房预约管理</span>
            </template>
          </el-menu-item>
          <el-menu-item index="/agreementManagement/agreement/agreement">
            <el-icon>
              <component is="Document"></component>
            </el-icon>
            <template #title>
              <span>租约管理</span>
            </template>
          </el-menu-item>
        </el-sub-menu>
        <el-menu-item index="/userManagement/userManagement">
          <el-icon>
            <component is="User"></component>
          </el-icon>
          <span>移动端用户管理</span>
        </el-menu-item>
      </el-menu>
    </el-scrollbar>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue'
import { useSettingsStore } from '@/store/modules/settings'
import { useUserStore } from '@/store/modules/user'
import { useRoute } from 'vue-router'
import Logo from '../Logo/index.vue'
export default defineComponent({
  components: {
    Logo,
  },
  setup() {
    const settingsStore = useSettingsStore()
    const userStore = useUserStore()
    const route = useRoute()
    const collapse = computed(() => settingsStore.collapse)
    const themeConfig = computed(() => settingsStore.themeConfig)
    const activeMenu = computed(() =>
      route.meta.activeMenu ? (route.meta.activeMenu as string) : route.path,
    )
    const userInfo = computed(() => userStore.userInfo)
    return {
      collapse,
      activeMenu,
      themeConfig,
      userInfo,
    }
  },
})
</script>

<style scoped lang="scss">
@mixin active {
  &:hover {
    color: $base-color-white;
  }

  &.is-active {
    color: $base-color-white;
    background-color: var(--el-color-primary) !important;
  }
}

.layout-sidebar-container {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  z-index: $base-z-index;
  width: $base-left-menu-width;
  height: 100vh;
  background: $base-menu-background;
  box-shadow: 2px 0 6px rgb(0 21 41 / 35%);
  transition: width $base-transition-time;

  &.is-collapse {
    width: $base-left-menu-width-min;
    border-right: 0;
  }

  :deep(.el-scrollbar__wrap) {
    overflow-x: hidden;

    .el-menu {
      border: 0;
    }

    .el-menu-item,
    .el-submenu__title {
      height: $base-menu-item-height;
      overflow: hidden;
      line-height: $base-menu-item-height;
      text-overflow: ellipsis;
      white-space: nowrap;
      vertical-align: middle;
    }

    .el-menu-item {
      @include active;
    }
  }
}
</style>
