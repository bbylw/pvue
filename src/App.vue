<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { categories } from './data/links.js';

const activeSectionId = ref('');
const currentYear = new Date().getFullYear();
const searchQuery = ref('');

const filteredCategories = computed(() => {
  const query = searchQuery.value.trim().toLowerCase();
  if (!query) return categories;

  return categories
    .map(cat => {
      const filteredLinks = cat.links.filter(
        link =>
          link.name.toLowerCase().includes(query) ||
          link.url.toLowerCase().includes(query)
      );
      return {
        ...cat,
        links: filteredLinks
      };
    })
    .filter(cat => cat.links.length > 0);
});

const clearSearch = () => {
  searchQuery.value = '';
};

const scrollToSection = (event, targetId) => {
  if (event) event.preventDefault();
  activeSectionId.value = targetId;
  const targetElement = document.getElementById(targetId);
  if (targetElement) {
    targetElement.scrollIntoView({ behavior: 'smooth' });
    const newUrl = `${window.location.protocol}//${window.location.host}${window.location.pathname}#${targetId}`;
    window.history.pushState({ path: newUrl }, '', newUrl);
  }
};

const handleHashChange = () => {
  const hash = window.location.hash;
  if (hash) {
    const targetId = hash.substring(1);
    const targetElement = document.getElementById(targetId);
    if (targetElement) {
      targetElement.scrollIntoView({ behavior: 'smooth' });
      activeSectionId.value = targetId;
    }
  }
};

onMounted(() => {
  window.addEventListener('hashchange', handleHashChange);
  handleHashChange();
});

onUnmounted(() => {
  window.removeEventListener('hashchange', handleHashChange);
});
</script>

<template>
  <div>
    <header>
      <h1>WebNav Hub</h1>
    </header>
    
    <div class="search-container">
      <div class="search-wrapper">
        <input
          type="text"
          v-model="searchQuery"
          class="search-input"
          placeholder="搜索导航网站 (名称或链接)..."
        />
        <i class="fa-solid fa-magnifying-glass search-icon-left"></i>
        <button
          v-if="searchQuery"
          @click="clearSearch"
          class="search-clear-btn"
          aria-label="清除搜索"
        >
          <i class="fa-solid fa-circle-xmark"></i>
        </button>
      </div>
    </div>

    <nav>
      <ul>
        <li v-for="cat in filteredCategories" :key="cat.id">
          <a
            :href="'#' + cat.id"
            :class="{ active: activeSectionId === cat.id }"
            @click="scrollToSection($event, cat.id)"
          >
            {{ cat.title }}
          </a>
        </li>
      </ul>
    </nav>

    <main>
      <div v-if="filteredCategories.length === 0" class="no-results">
        <i class="fa-solid fa-face-frown-open"></i>
        <p>没有找到相关网站，换个关键词试试吧~</p>
      </div>

      <section v-for="cat in filteredCategories" :key="cat.id" :id="cat.id">
        <h2 class="category-title">{{ cat.title }}</h2>
        <div class="link-grid">
          <div v-for="link in cat.links" :key="link.url" class="link-card">
            <a
              :href="link.url"
              target="_blank"
              rel="noopener noreferrer"
              :aria-label="link.name"
            ></a>
            <i :class="link.icon"></i>
            <h3>{{ link.name }}</h3>
          </div>
        </div>
      </section>
    </main>

    <footer>
      <p>© {{ currentYear }} WebNav Hub. 保留所有权利。</p>
      <nav>
        <a href="#">隐私政策</a>
        <a href="#">使用条款</a>
        <a href="#">联系我们</a>
      </nav>
    </footer>
  </div>
</template>
