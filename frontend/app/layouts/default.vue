<template>
  <div class="min-h-screen bg-gradient-to-b from-slate-950 via-slate-900 to-cyan-950 text-emerald-100">
    <div class="mx-auto flex min-h-screen w-full max-w-[1800px] flex-col p-2 md:p-4">
      <div class="relative flex min-h-0 flex-1 flex-col overflow-hidden border border-cyan-500/70 bg-slate-950/90">
        <div class="pointer-events-none absolute inset-0 bg-gradient-to-b from-cyan-900/30 via-slate-900/0 to-slate-900/60"></div>
        <Header />
        <main class="relative flex min-h-0 flex-1 overflow-auto">
          <div class="mx-auto flex w-full max-w-[1500px] flex-1 p-3 md:p-6">
            <div class="w-full md:aspect-video md:min-h-full">
              <slot />
            </div>
          </div>
        </main>
        <Footer />
      </div>
    </div>
  </div>
</template>
