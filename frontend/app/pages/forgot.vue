<template>
  <div class="flex min-h-full items-center justify-center px-2 py-4 md:px-4 md:py-6">
    <section class="w-full max-w-2xl overflow-hidden border border-emerald-500/70 bg-slate-900/90 p-6 md:p-8">
      <p class="text-xs font-semibold uppercase tracking-[0.22em] text-emerald-300">Recovery Node</p>
      <h2 class="mt-2 text-3xl font-black uppercase tracking-wide text-cyan-200">Forgot Password</h2>
      <p class="mt-3 text-sm leading-relaxed text-emerald-100/90">
        Password reset is not connected yet. This module can be wired to a mail token flow later.
      </p>
      <NuxtLink
        to="/login"
        class="mt-6 block border border-cyan-400 bg-cyan-900/35 px-4 py-3 text-center text-sm font-semibold uppercase tracking-wider text-cyan-200 hover:bg-cyan-800/55"
      >
        Back To Login
      </NuxtLink>
    </section>
  </div>
</template>
