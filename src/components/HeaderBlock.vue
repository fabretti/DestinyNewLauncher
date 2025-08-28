<template>
  <header class="main-header">
    <nav class="nav">
      <ul class="nav-list">
        <li v-for="item in navItems" :key="item.id" class="nav-item">
          <router-link class="text-body-20" :to="item.href">
            <span>{{ item.name }}</span>
          </router-link>
        </li>
      </ul>
      <div class="online-info text-body-18">
        <IconBase name="Online" />
        <span>В сети:</span>
        <span class="online-count">2 200</span>
      </div>
      <div class="user-info text-body-18">
        <IconBase name="Profile" />
        <span>destiny@mailr.ru</span>
      </div>
    </nav>
  </header>
</template>

<script setup lang="ts">
import IconBase from '@/shared/IconBase.vue'

const navItems = [
  { id: 1, icon: 'Home', href: '/', name: 'Игра' },
  { id: 2, icon: 'Ranking', href: '/rating', name: 'Новости' },
  { id: 3, icon: 'Shop', href: '/shop', name: 'Магазин' },
]
// Здесь можно добавить логику, если потребуется
</script>

<style lang="scss">
.main-header {
  position: relative;
  width: 100%;
  height: 64px;
  display: flex;
  align-items: center;
  box-sizing: border-box;
  z-index: 3;
  border-bottom: 0.5px solid rgba(255, 255, 255, 0.2);
  .nav {
    max-width: 1280px;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: 0 auto;
  }
  .nav-list {
    height: 100%;
    display: flex;
    gap: 32px;
    list-style: none;
    margin: 0;
    padding: 0;
    .nav-item {
      display: flex;
      align-items: center;
      gap: 8px;
      cursor: pointer;
      transition: opacity 0.2s;
      border-right: 0.5px solid rgba(255, 255, 255, 0.2);
      padding-right: 32px;
      .router-link-active {
        span {
          text-decoration: underline;
          text-underline-offset: 10px;
        }
      }
    }
  }
  .online-info {
    display: flex;
    align-items: center;
    gap: 4px;
  }
  .user-info {
    height: 100%;
    display: flex;
    align-items: center;
    border-left: 0.5px solid rgba(255, 255, 255, 0.2);
    padding-left: 26px;
  }
}
</style>
