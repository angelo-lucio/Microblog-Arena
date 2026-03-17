<template>
  <div class="flex min-h-full items-center justify-center px-2 py-4 md:px-4 md:py-6">
    <section class="grid w-full max-w-6xl grid-cols-1 overflow-hidden border border-cyan-500/70 bg-slate-900/90 xl:grid-cols-[1.1fr_0.9fr]">
      <div class="border-b border-cyan-800/60 bg-gradient-to-br from-cyan-950/60 via-slate-900 to-slate-900 p-7 xl:border-b-0 xl:border-r xl:p-10">
        <p class="text-xs font-semibold uppercase tracking-[0.3em] text-cyan-300">Microblog Arena</p>
        <h1 class="mt-3 text-4xl font-black uppercase tracking-wider text-emerald-200 md:text-5xl">Enter The Feed</h1>
        <p class="mt-4 max-w-xl text-sm leading-relaxed text-cyan-100/90 md:text-base">
          Social timeline with real-time moderation, AI correction hints and a competitive cyber style interface.
        </p>
        <div class="mt-6 grid grid-cols-1 gap-3 sm:grid-cols-2">
          <div class="border border-emerald-700/70 bg-slate-950/60 p-3 text-xs text-emerald-200">Live post stream</div>
          <div class="border border-cyan-700/70 bg-slate-950/60 p-3 text-xs text-cyan-200">AI safety pipeline</div>
        </div>
      </div>

      <div class="p-7 xl:p-10">
        <p class="text-xs font-semibold uppercase tracking-[0.2em] text-emerald-300">Launch</p>
        <div class="mt-4 space-y-3">
          <NuxtLink
            to="/login"
            class="block border border-emerald-400 bg-emerald-900/35 px-4 py-3 text-center text-sm font-semibold uppercase tracking-wider text-emerald-200 hover:bg-emerald-800/55"
          >
            Login
          </NuxtLink>
          <NuxtLink
            to="/register"
            class="block border border-cyan-400 bg-cyan-900/35 px-4 py-3 text-center text-sm font-semibold uppercase tracking-wider text-cyan-200 hover:bg-cyan-800/55"
          >
            Register
          </NuxtLink>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
import { useRouter } from "vue-router";

const auth = useAuth();
const router = useRouter();

onMounted(() => {
  if (auth.isAuthenticated()) {
    router.push("/feed");
  }
});
</script>
