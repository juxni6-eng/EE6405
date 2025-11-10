<template>
  <div class="layout">
    <nav class="nav">
      <a class="brand" href="/">EE6405 C20</a>
      <div class="links">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/dashboard">Dashboard</RouterLink>
      </div>
    </nav>
    <router-view />
    <footer class="foot">
      <small>© {{ year }} NTU EEE EE6405 C20, All rights reserved.</small>
    </footer>
  </div>
</template>

<script setup>
import { RouterLink } from 'vue-router'
const year = new Date().getFullYear()
</script>

<style>
:root{ font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Noto Sans, Ubuntu, Cantarell, Helvetica Neue, Arial, "Apple Color Emoji", "Segoe UI Emoji"; }
*{ box-sizing: border-box; }
body{ margin:0; }
.layout{ min-height:100vh; display:flex; flex-direction:column; }
.nav{ display:flex; align-items:center; justify-content:space-between; padding: 12px 18px; border-bottom:1px solid #eee; position: sticky; top:0; background:#fff; z-index:10; }
.brand{ font-weight:800; text-decoration:none; color:#111827; }
.links a{ margin-left: 12px; text-decoration:none; color:#374151; }
.links a.router-link-active{ color:#111827; font-weight:600; }
.foot{ margin-top:auto; padding: 24px 18px; text-align:center; color:#6b7280; border-top: 1px solid #eee; }
</style>
