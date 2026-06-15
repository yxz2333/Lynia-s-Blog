<script setup>
import { onMounted, ref } from 'vue'

// ======== giscus 配置 ========
// 1. 仓库 Settings → 开启 Discussions
// 2. 安装 giscus App: https://github.com/apps/giscus
// 3. 访问 https://giscus.app/zh-CN 获取 repo-id 和 category-id
const GISCUS_CONFIG = {
  repo: 'yxz2333/yxz2333.github.io',
  repoId: 'R_kgDOS6sjkQ',
  category: 'General',
  categoryId: 'DIC_kwDOS6sjkc4C_MM9',
  mapping: 'pathname',
  reactionsEnabled: '1',
  emitMetadata: '0',
  inputPosition: 'bottom',
  theme: 'preferred_color_scheme',
  lang: 'zh-CN',
}

const configured = ref(false)
const loading = ref(true)

onMounted(() => {
  const hasId = GISCUS_CONFIG.repoId && !GISCUS_CONFIG.repoId.includes('xxxx')
  if (!hasId) { loading.value = false; return }
  configured.value = true

  const script = document.createElement('script')
  script.src = 'https://giscus.app/client.js'
  script.setAttribute('data-repo', GISCUS_CONFIG.repo)
  script.setAttribute('data-repo-id', GISCUS_CONFIG.repoId)
  script.setAttribute('data-category', GISCUS_CONFIG.category)
  script.setAttribute('data-category-id', GISCUS_CONFIG.categoryId)
  script.setAttribute('data-mapping', GISCUS_CONFIG.mapping)
  script.setAttribute('data-reactions-enabled', GISCUS_CONFIG.reactionsEnabled)
  script.setAttribute('data-emit-metadata', GISCUS_CONFIG.emitMetadata)
  script.setAttribute('data-input-position', GISCUS_CONFIG.inputPosition)
  script.setAttribute('data-theme', GISCUS_CONFIG.theme)
  script.setAttribute('data-lang', GISCUS_CONFIG.lang)
  script.setAttribute('crossorigin', 'anonymous')
  script.setAttribute('async', 'true')
  script.onload = () => { loading.value = false }

  const container = document.getElementById('giscus-container')
  if (container) container.appendChild(script)
})
</script>

<template>
  <div class="mt-16 pt-8 border-t border-white/5">
    <h3 class="text-xl font-bold text-gray-200 mb-8 flex items-center gap-2">
      <span class="text-accent">💬</span> 评论区
    </h3>

    <!-- giscus 容器 -->
    <div v-if="configured" class="glass-card rounded-2xl p-4 sm:p-6">
      <div v-if="loading" class="flex justify-center py-10">
        <div class="w-6 h-6 border-2 border-accent/20 border-t-accent rounded-full animate-spin"></div>
      </div>
      <div id="giscus-container"></div>
    </div>

    <!-- 未配置提示 -->
    <div v-else class="glass-card rounded-2xl p-6 text-center text-gray-500 text-sm space-y-2">
      <p>💡 评论功能使用 <a href="https://giscus.app/zh-CN" target="_blank" class="text-accent hover:underline">giscus</a> 接入，基于 GitHub Discussions。</p>
      <ol class="text-xs text-left max-w-md mx-auto space-y-1 list-decimal">
        <li>仓库 Settings → 开启 <strong>Discussions</strong></li>
        <li>安装 <a href="https://github.com/apps/giscus" target="_blank" class="text-accent hover:underline">giscus App</a></li>
        <li>访问 giscus.app 获取 <code class="bg-white/5 px-1 rounded">repo-id</code> 和 <code class="bg-white/5 px-1 rounded">category-id</code></li>
        <li>填入 <code class="bg-white/5 px-1 rounded">CommentSection.vue</code> 的 GISCUS_CONFIG</li>
      </ol>
    </div>
  </div>
</template>
