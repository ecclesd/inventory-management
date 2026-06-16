<template>
  <aside class="sidebar" :class="{ 'sidebar--collapsed': collapsed }">
    <!-- Brand + toggle -->
    <div class="sidebar-brand">
      <template v-if="!collapsed">
        <span class="brand-dot"></span>
        <span class="brand-name">Inventory</span>
      </template>
      <button class="toggle-btn" @click="$emit('toggle')" :title="collapsed ? 'Expand sidebar' : 'Collapse sidebar'">
        <span v-html="collapsed ? icons.chevronRight : icons.chevronLeft"></span>
      </button>
    </div>

    <!-- Primary nav -->
    <nav class="sidebar-nav">
      <RouterLink
        v-for="item in navItems"
        :key="item.to"
        :to="item.to"
        class="nav-item"
        :data-label="item.label"
        active-class="active"
        exact-active-class="active"
      >
        <span class="nav-icon" v-html="item.icon"></span>
        <span v-if="!collapsed" class="nav-label">{{ item.label }}</span>
      </RouterLink>
    </nav>

    <!-- Footer: language switcher + profile menu -->
    <div class="sidebar-footer">
      <div class="footer-controls" :class="{ 'footer-controls--collapsed': collapsed }">
        <LanguageSwitcher v-if="!collapsed" class="footer-lang" />
        <ProfileMenu
          class="footer-profile"
          @show-profile-details="$emit('show-profile-details')"
          @show-tasks="$emit('show-tasks')"
        />
      </div>
    </div>
  </aside>
</template>

<script setup>
import ProfileMenu from './ProfileMenu.vue'
import LanguageSwitcher from './LanguageSwitcher.vue'

defineProps({ collapsed: { type: Boolean, default: false } })
defineEmits(['toggle', 'show-profile-details', 'show-tasks'])

const icons = {
  chevronLeft:  `<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 18 9 12 15 6"/></svg>`,
  chevronRight: `<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 18 15 12 9 6"/></svg>`,
  home:      `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 01-2 2H5a2 2 0 01-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>`,
  inventory: `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="3" width="20" height="14" rx="2"/><line x1="8" y1="21" x2="16" y2="21"/><line x1="12" y1="17" x2="12" y2="21"/></svg>`,
  orders:    `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"/><line x1="8" y1="12" x2="21" y2="12"/><line x1="8" y1="18" x2="21" y2="18"/><line x1="3" y1="6" x2="3.01" y2="6"/><line x1="3" y1="12" x2="3.01" y2="12"/><line x1="3" y1="18" x2="3.01" y2="18"/></svg>`,
  spending:  `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 000 7h5a3.5 3.5 0 010 7H6"/></svg>`,
  demand:    `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="23 6 13.5 15.5 8.5 10.5 1 18"/><polyline points="17 6 23 6 23 12"/></svg>`,
  reports:   `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>`,
  backlog:   `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="9" y1="13" x2="15" y2="13"/><line x1="9" y1="17" x2="15" y2="17"/></svg>`,
}

const navItems = [
  { to: '/',          label: 'Dashboard', icon: icons.home      },
  { to: '/inventory', label: 'Inventory', icon: icons.inventory },
  { to: '/orders',    label: 'Orders',    icon: icons.orders    },
  { to: '/spending',  label: 'Spending',  icon: icons.spending  },
  { to: '/demand',    label: 'Demand',    icon: icons.demand    },
  { to: '/reports',   label: 'Reports',   icon: icons.reports   },
  { to: '/backlog',   label: 'Backlog',   icon: icons.backlog   },
]
</script>

<style scoped>
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: var(--sidebar-width);
  background: var(--sidebar-bg);
  border-right: var(--sidebar-border);
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  overflow-x: hidden;
  z-index: 100;
  /* smooth width transition */
  transition: width 0.2s ease;
}

/* ── Brand ── */
.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 20px 16px 16px;
  border-bottom: 1px solid #e2e8f0;
  min-height: 65px;
}
.sidebar--collapsed .sidebar-brand {
  justify-content: center;
  padding: 20px 0 16px;
}

.brand-dot {
  width: 28px;
  height: 28px;
  border-radius: 8px;
  background: var(--accent);
  flex-shrink: 0;
}
.brand-name {
  flex: 1;
  color: var(--sidebar-text-strong);
  font-size: 15px;
  font-weight: 700;
  letter-spacing: -0.01em;
  white-space: nowrap;
  overflow: hidden;
}

.toggle-btn {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  background: transparent;
  color: var(--sidebar-text);
  cursor: pointer;
  transition: background 0.15s, color 0.15s;
  padding: 0;
}
.toggle-btn:hover {
  background: var(--sidebar-hover-bg);
  color: var(--sidebar-text-strong);
}

/* ── Nav ── */
.sidebar-nav {
  flex: 1;
  padding: 12px 8px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.sidebar--collapsed .sidebar-nav {
  padding: 12px 6px;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 9px 10px;
  border-radius: 8px;
  border-left: 3px solid transparent;
  color: var(--sidebar-text);
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  transition: background 0.15s ease, color 0.15s ease;
  white-space: nowrap;
  position: relative;
}
.nav-item:hover {
  background: var(--sidebar-hover-bg);
  color: var(--sidebar-text-strong);
}
.nav-item.active {
  background: var(--sidebar-active-bg);
  color: var(--sidebar-active-text);
  border-left: 3px solid var(--accent);
  padding-left: 7px;
}

/* icon-only layout */
.sidebar--collapsed .nav-item {
  justify-content: center;
  padding: 9px 6px;
  gap: 0;
}
.sidebar--collapsed .nav-item.active {
  padding-left: 3px; /* compensate for 3px border in 48px space */
}

/* tooltip shown on hover in collapsed mode */
.sidebar--collapsed .nav-item::after {
  content: attr(data-label);
  position: absolute;
  left: calc(100% + 10px);
  top: 50%;
  transform: translateY(-50%);
  background: #0f172a;
  color: #f8fafc;
  font-size: 12px;
  font-weight: 500;
  padding: 5px 10px;
  border-radius: 6px;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.15s ease;
  z-index: 200;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
}
.sidebar--collapsed .nav-item:hover::after {
  opacity: 1;
}

.nav-icon {
  flex-shrink: 0;
  display: flex;
  align-items: center;
}
.nav-label {
  overflow: hidden;
  text-overflow: ellipsis;
}

/* ── Footer ── */
.sidebar-footer {
  padding: 8px 8px 16px;
  border-top: 1px solid #e2e8f0;
}

.footer-controls {
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 4px 0;
}

.footer-controls--collapsed {
  align-items: center;
}

/* ProfileMenu fills width in expanded mode */
.footer-profile {
  width: 100%;
}

/* Override ProfileMenu's internal button to blend into sidebar */
.footer-profile :deep(.profile-button) {
  width: 100%;
  border: none;
  background: transparent;
  border-radius: 8px;
  padding: 8px 10px;
}
.footer-profile :deep(.profile-button):hover {
  background: var(--sidebar-hover-bg);
  border: none;
}

/* Dropdown opens upward so it isn't clipped at the bottom of the sidebar */
.footer-profile :deep(.dropdown-menu) {
  bottom: calc(100% + 6px);
  top: auto;
  left: 0;
  right: auto;
}

/* LanguageSwitcher in sidebar footer */
.footer-lang {
  width: 100%;
}
.footer-lang :deep(.language-button) {
  width: 100%;
  border: none;
  background: transparent;
  border-radius: 8px;
  padding: 8px 10px;
}
.footer-lang :deep(.language-button):hover {
  background: var(--sidebar-hover-bg);
  border: none;
}
.footer-lang :deep(.dropdown-menu) {
  bottom: calc(100% + 6px);
  top: auto;
  left: 0;
  right: auto;
}
</style>
