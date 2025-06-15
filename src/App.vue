<script setup lang="ts">
import TopVideoHeader from './components/TopVideoHeader.vue'
import SidebarProfile from './components/SidebarProfile.vue'
import SidebarNav from './components/SidebarNav.vue'
import SidebarExtra from './components/SidebarExtra.vue'
import UserAvatar from './components/UserAvatar.vue'
import { RouterView, useRoute } from 'vue-router'
import { computed } from 'vue'

const route = useRoute()
const isLoggedIn = computed(() => localStorage.getItem('isLoggedIn') === 'true')
const isAuthPage = computed(() => ['login', 'register'].includes(route.name as string))
</script>

<template>
  <div class="app">
    <div v-if="isAuthPage">
      <RouterView />
    </div>
    <div v-else class="main-layout">
      <TopVideoHeader />
      <div class="content-row">
        <aside class="sidebar-left">
          <SidebarProfile />
          <SidebarNav />
        </aside>
        <main class="main-content">
          <RouterView />
        </main>
        <aside class="sidebar-right">
          <SidebarExtra />
        </aside>
      </div>
      <div class="user-avatar-container" v-if="isLoggedIn">
        <UserAvatar />
      </div>
    </div>
  </div>
</template>

<style>
html,
body,
#app {
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>

<style scoped>
.app {
  width: 100%;
  min-height: 100vh;
}

.main-layout {
  width: 100%;
  min-height: 100vh;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  padding-top: 0;
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
}

.content-row {
  display: flex;
  width: 100%;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  position: relative;
  z-index: 2;
  overflow: visible;
  flex: 1;
  height: calc(100vh - 56px);
}

.sidebar-left {
  width: 260px;
  margin: 0;
  align-self: stretch;
  border-radius: 0 16px 16px 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  flex-shrink: 0;
}

.main-content {
  flex: 1 1 0;
  min-width: 0;
  margin: 0 16px;
  position: relative;
  z-index: 2;
}

.sidebar-right {
  width: 300px;
  margin: 0;
  align-self: stretch;
  border-radius: 16px 0 0 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  flex-shrink: 0;
}

.user-avatar-container {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 99999;
}

@media (max-width: 1200px) {
  .sidebar-left,
  .sidebar-right {
    width: 200px;
  }
}

@media (max-width: 900px) {
  .main-layout {
    padding-top: 50vh;
  }
  .content-row {
    flex-direction: column;
    align-items: stretch;
    width: 100%;
  }
  .sidebar-left,
  .sidebar-right {
    width: 100%;
    margin: 0 0 16px 0;
    border-radius: 16px;
  }
  .main-content {
    margin: 0 0 16px 0;
  }
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
  
  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>

