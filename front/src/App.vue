<template>
  <stepin-view
    system-name="Stepin"
    logo-src="@/assets/vite.svg"
    :class="`${contentClass}`"
    :user="user"
    :navMode="navigation"
    :useTabs="useTabs"
    :themeList="themeList"
    v-model:show-setting="showSetting"
    v-model:theme="theme"
    @themeSelect="configTheme"
  >
    <template #headerActions>
      <HeaderActions @showSetting="showSetting = true" />
    </template>
    <template #pageFooter>
      <PageFooter />
    </template>
    <template #themeEditorTab>
      <a-tab-pane tab="其它" key="other">
        <Setting />
      </a-tab-pane>
    </template>
  </stepin-view>
  <login-modal :unless="['/login']" />
</template>

<script lang="ts" setup>
  import { reactive, ref } from 'vue';
  import PageFooter from '@/components/layout/PageFooter.vue';
  import { LoginModal } from '@/pages/login';
  import { useAccountStore, useMenuStore, useSettingStore, storeToRefs } from '@/store';
  import { useRouter } from 'vue-router';
  import { useAuthStore } from '@/plugins/auth-plugin';
  import { configTheme, themeList } from '@/theme';
  import HeaderActions from './components/layout/HeaderActions.vue';
  import Setting from './components/setting/Setting.vue';
  import avatar from '@/assets/avatar.png';

  const { logout, profile } = useAccountStore();
  const { setAuthorities } = useAuthStore();

  // onMounted(()=>{console.log(1)})

  // 获取个人信息
  profile()
    .then((response) => {
      const { permissions, account } = response;
      setAuthorities(permissions);
      user.name = account.name;
      user.avatar = account.avatar;
    })
    .catch((e) => {
      console.log(e);
    });

  const showSetting = ref(false);
  const router = useRouter();

  useMenuStore().getMenuList();

  const { navigation, useTabs, theme, contentClass } = storeToRefs(useSettingStore());

  const user = reactive({
    name: '',
    avatar: avatar,
    menuList: [
      { title: '设置', key: 'setting', icon: 'SettingOutlined', onClick: () => (showSetting.value = true) },
      { type: 'divider' },
      {
        title: '退出登录',
        key: 'logout',
        icon: 'LogoutOutlined',
        onClick: () => logout().then(() => router.push('/login')),
      },
    ],
  });
</script>

<style lang="less">
  ::-webkit-scrollbar {
    width: 4px;
    border-radius: 4px;
    background-color: theme('colors.primary.500');
  }

  ::-webkit-scrollbar-thumb {
    border-radius: 4px;
    background-color: theme('colors.primary.400');

    &:hover {
      background-color: theme('colors.primary.500');
    }
  }

  ::-webkit-scrollbar-track {
    box-shadow: inset 0 0 1px rgba(0, 0, 0, 0);
    border-radius: 4px;
    background: theme('backgroundColor.layout');
  }

  html {
    height: 100vh;
    overflow-y: hidden;
  }

  body {
    margin: 0;
    height: 100vh;
    overflow-y: hidden;
  }
  .stepin-img-checkbox {
    @apply transition-transform;
    &:hover {
      @apply scale-105 ~"-translate-y-[2px]";
    }
    img {
      @apply shadow-low rounded-md transition-transform;
    }
  }
</style>
