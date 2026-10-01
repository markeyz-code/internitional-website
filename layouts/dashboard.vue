
<template>
  <div class="h-screen w-full flex bg-[#F8FAFC] overflow-hidden font-sans">
    <!-- Sidebar -->
    <aside class="w-64 bg-[#0F172A] text-slate-300 flex-shrink-0 hidden lg:flex flex-col relative z-20 transition-all duration-300 border-r border-slate-800">
      <!-- Logo Area -->
      <div class="h-16 flex items-center px-6 border-b border-white/10 relative overflow-hidden">
        <NuxtLink to="/dashboard/overview" class="flex items-center gap-3 relative z-10">
          <img src="~/assets/logo-icon.png" class="h-6 w-auto" alt="UniVerse Logo" />
          <span class="text-lg font-bold text-white tracking-tight">Portal</span>
        </NuxtLink>
      </div>
      
      <!-- Navigation -->
      <div class="flex-1 overflow-y-auto py-6 px-4 custom-scrollbar">
        <p class="text-[10px] font-bold text-slate-500 uppercase tracking-widest mb-3 px-3">Main Menu</p>
        <nav class="space-y-1">
          <NuxtLink to="/dashboard/overview" class="nav-item group" active-class="active-nav">
            <div class="nav-icon"><LayoutDashboard class="w-4 h-4" /></div>
            <span class="font-medium text-sm">Overview</span>
          </NuxtLink>
          <NuxtLink to="/dashboard/vault" class="nav-item group" active-class="active-nav">
            <div class="nav-icon"><Folder class="w-4 h-4" /></div>
            <span class="font-medium text-sm">The Vault</span>
          </NuxtLink>
          <NuxtLink to="/dashboard/mentorship" class="nav-item group" active-class="active-nav">
            <div class="nav-icon"><Users class="w-4 h-4" /></div>
            <span class="font-medium text-sm">Mentorship Matcher</span>
          </NuxtLink>
          <NuxtLink to="/dashboard/career" class="nav-item group" active-class="active-nav">
            <div class="nav-icon"><Briefcase class="w-4 h-4" /></div>
            <span class="font-medium text-sm">Career Hub</span>
          </NuxtLink>
          <NuxtLink to="/dashboard/notifications" class="nav-item group" active-class="active-nav">
            <div class="nav-icon"><Bell class="w-4 h-4" /></div>
            <span class="font-medium text-sm">Notifications</span>
            <span v-if="unreadCount > 0" class="ml-auto px-1.5 py-0.2 min-w-[18px] text-center rounded-full text-[10px] font-extrabold bg-red-500 text-white animate-pulse">
              {{ unreadCount > 99 ? '99+' : unreadCount }}
            </span>
          </NuxtLink>
          
          <div class="pt-5 pb-1">
            <p class="text-[10px] font-bold text-slate-500 uppercase tracking-widest mb-3 px-3">Account</p>
          </div>
          
          <NuxtLink to="/dashboard/pricing" class="nav-item group" active-class="active-nav">
            <div class="nav-icon"><CreditCard class="w-4 h-4" /></div>
            <span class="font-medium text-sm">Subscription</span>
          </NuxtLink>
          <NuxtLink to="/dashboard/events" class="nav-item group" active-class="active-nav">
            <div class="nav-icon"><Calendar class="w-4 h-4" /></div>
            <span class="font-medium text-sm">Events</span>
          </NuxtLink>
        </nav>
      </div>

      <!-- User Profile Profile -->
      <div class="mt-auto p-3 m-3 bg-white/5 border border-white/10 rounded-xl backdrop-blur-sm">
        <div class="flex items-center gap-3 mb-3">
          <div class="w-8 h-8 rounded-full bg-[#1f4e70] flex items-center justify-center text-white font-bold text-sm shadow-inner">
            {{ userInitial }}
          </div>
          <div class="flex-1 min-w-0">
            <p class="text-xs font-bold text-white truncate">{{ userDisplayName }}</p>
            <p class="text-[10px] text-slate-400 truncate">{{ user?.email || 'user@example.com' }}</p>
          </div>
        </div>
        <button @click="handleLogout" class="w-full flex items-center justify-center gap-2 py-2 rounded-lg bg-white/10 hover:bg-red-500/20 hover:text-red-400 text-slate-300 text-xs font-medium transition-all duration-300 cursor-pointer">
          <LogOut class="w-3.5 h-3.5" />
          Sign Out
        </button>
      </div>
    </aside>

    <!-- Main Wrapper -->
    <div class="flex-1 flex flex-col min-w-0 bg-[#F8FAFC]">
      
      <!-- Header -->
      <header class="h-16 bg-white border-b border-slate-200 flex items-center justify-between px-6 sticky top-0 z-30">
        <!-- Mobile Menu Toggle -->
        <div class="flex items-center gap-4">
          <button class="lg:hidden p-2 text-slate-600 hover:bg-slate-100 rounded-xl transition-colors">
            <Menu class="w-5 h-5" />
          </button>
          
          <div class="hidden md:flex items-center bg-slate-50 rounded-full px-3 py-1.5 w-64 border border-slate-200 focus-within:border-[#1f4e70] transition-all">
            <Search class="w-4 h-4 text-slate-400 mr-2" />
            <input type="text" placeholder="Search anything..." class="bg-transparent border-none outline-none w-full text-xs text-slate-700 placeholder-slate-400" />
          </div>
        </div>

        <div class="flex items-center gap-4">
          <NuxtLink
            to="/dashboard/notifications"
            class="w-9 h-9 rounded-full border border-slate-200 flex items-center justify-center text-slate-500 hover:bg-slate-50 hover:text-[#1f4e70] transition-colors relative"
            title="Notifications"
          >
            <Bell class="w-4 h-4" />
            <span
              v-if="unreadCount > 0"
              class="absolute -top-1 -right-1 min-w-[18px] h-[18px] px-1 bg-red-500 text-white rounded-full text-[10px] font-extrabold flex items-center justify-center ring-2 ring-white animate-pulse"
            >
              {{ unreadCount > 99 ? '99+' : unreadCount }}
            </span>
          </NuxtLink>
        </div>
      </header>
      
      <!-- Page Content -->
      <main class="flex-1 overflow-auto p-4 lg:p-6">
        <div class="max-w-6xl mx-auto">
          <slot />
        </div>
      </main>
      
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import { Folder, Users, Briefcase, LogOut, Menu, CreditCard, LayoutDashboard, Calendar, Search, Bell } from 'lucide-vue-next';
import { useAuth } from '@/composables/core/useAuth';
import { useNotifications } from '@/composables/modules/notifications/useNotifications';

const router = useRouter();
const { user, clearAuth, initAuth, fetchUserProfile } = useAuth();
const { unreadCount, initWebSocket, fetchUnreadCount } = useNotifications();

onMounted(async () => {
  initAuth();
  if (!user.value?.firstName) {
    await fetchUserProfile();
  }
  initWebSocket();
  await fetchUnreadCount();
});

const userDisplayName = computed(() => {
  if (user.value?.firstName) {
    return `${user.value.firstName} ${user.value.lastName || ''}`.trim();
  }
  if (user.value?.email) {
    return user.value.email.split('@')[0];
  }
  return 'Intern Member';
});

const userInitial = computed(() => {
  if (user.value?.firstName) return user.value.firstName.charAt(0).toUpperCase();
  if (user.value?.email) return user.value.email.charAt(0).toUpperCase();
  return 'U';
});

const { confirm } = useCustomModal();

const handleLogout = async () => {
  const confirmed = await confirm({
    title: 'Sign Out',
    message: 'Are you sure you want to log out of your session?',
    confirmText: 'Sign Out',
    cancelText: 'Cancel',
    type: 'danger',
  });
  if (confirmed) {
    clearAuth();
    router.push('/login');
  }
};
</script>

<style scoped>
.brand {
  --tw-text-opacity: 1;
  color: rgb(31 78 112 / var(--tw-text-opacity));
}
.bg-brand {
  --tw-bg-opacity: 1;
  background-color: rgb(31 78 112 / var(--tw-bg-opacity));
}
.border-brand {
  --tw-border-opacity: 1;
  border-color: rgb(31 78 112 / var(--tw-border-opacity));
}

.nav-item {
  @apply flex items-center gap-3 px-3 py-2.5 rounded-lg text-slate-400 hover:text-white hover:bg-white/5 transition-all duration-300;
}
.nav-icon {
  @apply flex items-center justify-center transition-transform duration-300 group-hover:scale-110;
}
.active-nav {
  @apply bg-[#1f4e70]/20 text-white font-bold border border-[#1f4e70]/30 shadow-[inset_0px_0px_10px_rgba(31,78,112,0.1)];
}
.active-nav .nav-icon {
  @apply text-white;
}
.custom-scrollbar::-webkit-scrollbar {
  width: 4px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  background: transparent;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
}
</style>
