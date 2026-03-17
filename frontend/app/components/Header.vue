<template>
  <header class="relative border-b border-cyan-500/70 bg-slate-900/95 text-cyan-100">
    <div class="mx-auto w-full max-w-[1500px] px-4 py-3 md:px-6">
      <div class="flex flex-wrap items-center justify-between gap-3">
        <div class="flex items-center gap-3 md:gap-4">
          <h1 class="text-xl font-black tracking-widest text-cyan-300 md:text-2xl">MICROBLOG ARENA</h1>
          <div
            v-if="loggedIn && user"
            class="flex items-center gap-2 border border-emerald-400/80 bg-slate-800/90 px-2 py-1"
          >
            <img
              :src="avatarUrl(user.username)"
              :alt="`Avatar ${user.username}`"
              class="h-8 w-8 border border-cyan-400 object-cover"
            />
            <span class="text-xs font-semibold uppercase tracking-wide text-emerald-300 md:text-sm">
              {{ user.username }}
            </span>
          </div>
        </div>
        <nav class="flex flex-wrap items-center gap-2 text-xs font-semibold uppercase tracking-wider md:text-sm">
          <NuxtLink v-if="!loggedIn" to="/login" class="border border-emerald-400 bg-emerald-900/30 px-3 py-1 hover:bg-emerald-800/50">
            Login
          </NuxtLink>
          <NuxtLink v-if="!loggedIn" to="/register" class="border border-cyan-400 bg-cyan-900/30 px-3 py-1 hover:bg-cyan-800/50">
            Register
          </NuxtLink>
          <NuxtLink v-if="loggedIn" to="/feed" class="border border-emerald-400 bg-emerald-900/30 px-3 py-1 hover:bg-emerald-800/50">
            Feed
          </NuxtLink>
          <NuxtLink v-if="loggedIn" to="/posts/create" class="border border-cyan-400 bg-cyan-900/30 px-3 py-1 hover:bg-cyan-800/50">
            New Post
          </NuxtLink>
          <button
            v-if="loggedIn"
            @click="handleLogout"
            class="border border-red-400 bg-red-900/30 px-3 py-1 text-red-200 hover:bg-red-800/50"
          >
            Exit
          </button>
        </nav>
      </div>
    </div>
  </header>
</template>

<script setup lang="ts">
import { useAuth } from "../composables/useAuth";
import { useRouter } from "vue-router";
import { computed } from "vue";

const auth = useAuth();
const router = useRouter();

// Reactive: reads from the module-level singleton ref in useAuth
const user = computed(() => auth.getUser());
const loggedIn = computed(() => !!user.value);

const handleLogout = () => {
  auth.logout();
  router.push("/login");
};

const avatarUrl = (username: string) => {
  const safeSeed = encodeURIComponent(username || "pilot");
  return `https://api.dicebear.com/9.x/pixel-art/svg?seed=${safeSeed}`;
};
</script>
