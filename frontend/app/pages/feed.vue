<template>
  <div class="flex min-h-full w-full flex-col">
    <div class="mb-4 flex flex-wrap items-center justify-between gap-3 border border-cyan-500/70 bg-slate-900/70 p-4 md:mb-6 md:p-5">
      <div>
        <h1 class="text-2xl font-black uppercase tracking-wider text-cyan-300 md:text-3xl">Live Feed</h1>
        <p class="text-xs uppercase tracking-wide text-emerald-300 md:text-sm">Arena stream with AI moderation</p>
      </div>
      <NuxtLink
        to="/posts/create"
        class="border border-emerald-400 bg-emerald-900/40 px-4 py-2 text-xs font-bold uppercase tracking-wider text-emerald-300 hover:bg-emerald-800/60 md:text-sm"
      >
        Create Post
      </NuxtLink>
    </div>

    <div class="grid flex-1 grid-cols-1 gap-4 pb-2 md:grid-cols-2 xl:grid-cols-3">
      <article
        v-for="p in posts"
        :key="p.id"
        class="flex min-h-[260px] flex-col border border-cyan-500/70 bg-slate-900/80 p-4"
      >
        <div class="mb-3 flex items-start justify-between gap-3 border-b border-slate-700 pb-3">
          <div class="flex items-center gap-3">
            <img
              :src="avatarUrl(ownerName(p))"
              :alt="`Avatar ${ownerName(p)}`"
              class="h-10 w-10 border border-cyan-400 object-cover"
            />
            <div>
              <p class="text-sm font-semibold uppercase tracking-wide text-cyan-200">{{ ownerName(p) }}</p>
              <p class="text-xs uppercase tracking-wide text-slate-400">Post #{{ p.id }} | UID {{ p.userId }}</p>
            </div>
          </div>
          <span :class="statusClass(p.sentiment)" class="border px-2 py-1 text-[11px] font-bold uppercase tracking-wide">
            {{ statusLabel(p.sentiment) }}
          </span>
        </div>

        <p class="mb-4 flex-1 whitespace-pre-wrap text-sm leading-relaxed text-emerald-100">{{ p.content }}</p>

        <div v-if="hasCorrection(p)" class="mb-3 border border-amber-400/70 bg-amber-950/30 p-2">
          <p class="text-[11px] font-bold uppercase tracking-wide text-amber-300">AI correction</p>
          <p class="mt-1 text-xs text-amber-100">{{ p.correction }}</p>
        </div>

        <div v-if="currentUser && currentUser.id === p.userId" class="mt-auto flex gap-2 pt-2">
          <NuxtLink
            :to="`/posts/${p.id}/edit`"
            class="flex-1 border border-cyan-400 bg-cyan-900/40 px-3 py-2 text-center text-xs font-semibold uppercase tracking-wide text-cyan-200 hover:bg-cyan-800/60"
          >
            Edit
          </NuxtLink>
          <button
            @click="deletePost(p.id)"
            class="flex-1 border border-red-400 bg-red-900/30 px-3 py-2 text-xs font-semibold uppercase tracking-wide text-red-200 hover:bg-red-800/50"
          >
            Delete
          </button>
        </div>
      </article>
    </div>

    <p v-if="posts.length === 0" class="border border-emerald-500 bg-slate-900/80 p-8 text-center text-sm font-semibold uppercase tracking-wide text-emerald-300">
      No posts available
    </p>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount } from "vue";
import { useRouter } from "vue-router";

type Sentiment = "pending" | "ok" | "negative" | "dangerous" | "error" | string | null | undefined;

type FeedPost = {
  id: number;
  content: string;
  userId: number;
  username?: string | null;
  authorName?: string | null;
  sentiment?: Sentiment;
  correction?: string | null;
};

const router = useRouter();
const auth = useAuth();
const { baseUrl, authFetch } = useApi();
const posts = ref<FeedPost[]>([]);
const currentUser = computed(() => auth.getUser());
let refreshTimer: ReturnType<typeof setInterval> | null = null;

const fetchPosts = async () => {
  try {
    const result = await authFetch<FeedPost[]>(`${baseUrl}/posts`);
    posts.value = result;
  } catch (e) {
    console.error(e);
  }
};

const startAutoRefresh = () => {
  if (refreshTimer) return;
  refreshTimer = setInterval(() => {
    void fetchPosts();
  }, 2500);
};

const stopAutoRefresh = () => {
  if (!refreshTimer) return;
  clearInterval(refreshTimer);
  refreshTimer = null;
};

const deletePost = async (id: number) => {
  if (!confirm("DELETE THIS POST?")) return;
  try {
    await authFetch(`${baseUrl}/posts/${id}`, { method: "DELETE" });
    posts.value = posts.value.filter((p) => p.id !== id);
  } catch (e) {
    console.error(e);
  }
};

const ownerName = (post: FeedPost) => post.username || post.authorName || `USER#${post.userId}`;

const avatarUrl = (username: string) => {
  const safeSeed = encodeURIComponent(username || "pilot");
  return `https://api.dicebear.com/9.x/pixel-art/svg?seed=${safeSeed}`;
};

const hasCorrection = (post: FeedPost) => !!post.correction && post.correction.trim().length > 0;

const statusLabel = (sentiment: Sentiment) => {
  if (!sentiment) return "Pending";
  const normalized = String(sentiment).toLowerCase();
  if (normalized === "ok") return "Clean";
  if (normalized === "negative") return "Needs Fact Check";
  if (normalized === "dangerous") return "Dangerous";
  if (normalized === "error") return "AI Retry Needed";
  if (normalized === "pending") return "Pending";
  return normalized;
};

const statusClass = (sentiment: Sentiment) => {
  const normalized = String(sentiment || "pending").toLowerCase();
  if (normalized === "ok") return "border-emerald-400 text-emerald-300 bg-emerald-900/30";
  if (normalized === "negative") return "border-yellow-400 text-yellow-300 bg-yellow-950/40";
  if (normalized === "dangerous") return "border-red-400 text-red-300 bg-red-950/40";
  if (normalized === "error") return "border-orange-400 text-orange-300 bg-orange-950/40";
  return "border-cyan-400 text-cyan-300 bg-cyan-950/40";
};

onMounted(() => {
  if (!auth.isAuthenticated()) {
    router.push("/login");
    return;
  }
  void fetchPosts();
  startAutoRefresh();
});

onBeforeUnmount(() => {
  stopAutoRefresh();
});
</script>
