<script setup>
import { ref } from 'vue'

const search = ref('')
const activeCat = ref('Tất cả')

const categories = [
  'Tất cả', 'Pop', 'Rock', 'Electronic', 'Cinematic', 'Jazz', 'R&B', 'Lofi'
]

const exploreProducts = [
  {
    id: 'exp-1',
    title: 'Neon Nights',
    artist: 'Alex Rivera',
    price: '2.450.000đ',
    cover: 'linear-gradient(135deg, #0e1a38 0%, #d12b7a 50%, #46e8d2 100%)',
    tag: 'BẢN QUYỀN GỐC'
  },
  {
    id: 'exp-2',
    title: 'Ethereal Dreams',
    artist: 'Luna Voce',
    price: '1.890.000đ',
    cover: 'linear-gradient(135deg, #1c3d5a 0%, #d89f71 50%, #152238 100%)',
    tag: ''
  },
  {
    id: 'exp-3',
    title: 'Urban Pulse',
    artist: 'The Beatmakers',
    price: '3.200.000đ',
    cover: 'linear-gradient(135deg, #090909 0%, #122a2a 50%, #48c6b7 100%)',
    tag: ''
  },
  {
    id: 'exp-4',
    title: 'Acoustic Soul',
    artist: 'Mia Tran',
    price: '950.000đ',
    cover: 'linear-gradient(135deg, #3d2a1d 0%, #b38b6d 50%, #1a1a1a 100%)',
    tag: 'BẢN QUYỀN GỐC'
  },
  {
    id: 'exp-5',
    title: 'Midnight Jazz',
    artist: 'The Quartet',
    price: '1.250.000đ',
    cover: 'linear-gradient(135deg, #111 0%, #2c3e50 50%, #000 100%)',
    tag: ''
  }
]
</script>

<template>
  <div class="explore-layout">
    <!-- SIDEBAR -->
    <aside class="explore-sidebar">
      <div class="sidebar-header">
        <h2>Explore</h2>
        <span>Marketplace</span>
      </div>

      <nav class="sidebar-nav">
        <a href="#" class="nav-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>
          Home
        </a>
        <a href="#" class="nav-item is-active">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="22 7 13.5 15.5 8.5 10.5 2 17"></polyline><polyline points="16 7 22 7 22 13"></polyline></svg>
          Trending
        </a>
        <a href="#" class="nav-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m12 2-2 4h-4l3 3-1 4 4-2 4 2-1-4 3-3h-4z"></path></svg>
          New Releases
        </a>
        <a href="#" class="nav-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="18" x="3" y="3" rx="2" ry="2"></rect><line x1="3" x2="21" y1="9" y2="9"></line><line x1="9" x2="9" y1="21" y2="9"></line></svg>
          Top Charts
        </a>
        <a href="#" class="nav-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m16 6 4 14"></path><path d="M12 6v14"></path><path d="M8 8v12"></path><path d="M4 4v16"></path></svg>
          My Library
        </a>
      </nav>

      <div class="sidebar-bottom">
        <button class="btn-upload">
          <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="12" x2="12" y1="5" y2="19"></line><line x1="5" x2="19" y1="12" y2="12"></line></svg>
          Upload Track
        </button>
        <a href="#" class="nav-item settings-link">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"></circle><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path></svg>
          Settings
        </a>
      </div>
    </aside>

    <!-- MAIN CONTENT -->
    <main class="explore-main">
      <div class="search-container">
        <input v-model="search" type="text" placeholder="Tìm kiếm bài hát, nghệ sĩ hoặc bản quyền..." class="main-search-input" />
      </div>

      <div class="filter-chips">
        <button 
          v-for="cat in categories" 
          :key="cat" 
          :class="['chip', { 'is-active': activeCat === cat }]"
          @click="activeCat = cat"
        >
          {{ cat }}
        </button>
      </div>

      <div class="section-header">
        <div>
          <h1 class="section-title">Xu hướng Marketplace</h1>
          <p class="section-subtitle">Hơn 50,000 bản quyền âm nhạc chất lượng cao dành cho dự án của bạn.</p>
        </div>
        <div class="view-toggles">
          <button class="toggle-btn is-active">
            <svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="7" height="7" x="3" y="3" rx="1"></rect><rect width="7" height="7" x="14" y="3" rx="1"></rect><rect width="7" height="7" x="14" y="14" rx="1"></rect><rect width="7" height="7" x="3" y="14" rx="1"></rect></svg>
          </button>
          <button class="toggle-btn">
            <svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" x2="21" y1="6" y2="6"></line><line x1="8" x2="21" y1="12" y2="12"></line><line x1="8" x2="21" y1="18" y2="18"></line><line x1="3" x2="3.01" y1="6" y2="6"></line><line x1="3" x2="3.01" y1="12" y2="12"></line><line x1="3" x2="3.01" y1="18" y2="18"></line></svg>
          </button>
        </div>
      </div>

      <div class="product-grid">
        <div v-for="p in exploreProducts" :key="p.id" class="exp-card">
          <div class="exp-cover" :style="{ background: p.cover }">
            <span v-if="p.tag" class="exp-badge">
              <svg viewBox="0 0 24 24" width="10" height="10" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" style="margin-right:3px"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path><polyline points="22 4 12 14.01 9 11.01"></polyline></svg>
              {{ p.tag }}
            </span>
          </div>
          <div class="exp-info">
            <div class="exp-title">{{ p.title }}</div>
            <div class="exp-artist">{{ p.artist }}</div>
            <div class="exp-bottom">
              <div class="exp-price">{{ p.price }}</div>
              <button class="btn-cart" aria-label="Thêm vào giỏ">
                <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 20a1 1 0 1 0 0 2 1 1 0 1 0 0-2z"></path><path d="M20 20a1 1 0 1 0 0 2 1 1 0 1 0 0-2z"></path><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path><line x1="12" y1="10" x2="12" y2="14"></line><line x1="10" y1="12" x2="14" y2="12"></line></svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- FIXED PLAYER -->
    <div class="fixed-player">
      <div class="player-left">
        <div class="p-cover" :style="{ background: exploreProducts[0].cover }"></div>
        <div class="p-info">
          <div class="p-title">{{ exploreProducts[0].title }}</div>
          <div class="p-artist">{{ exploreProducts[0].artist }}</div>
        </div>
      </div>
      
      <div class="player-center">
        <div class="p-controls">
          <button class="p-btn"><svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 18l-6-6 6-6"></path></svg></button>
          <button class="p-btn"><svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="19 20 9 12 19 4 19 20"></polygon><line x1="5" x2="5" y1="19" y2="5"></line></svg></button>
          <button class="p-btn-play"><svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor"><path d="M8 5v14l11-7z" /></svg></button>
          <button class="p-btn"><svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="5 4 15 12 5 20 5 4"></polygon><line x1="19" x2="19" y1="5" y2="19"></line></svg></button>
          <button class="p-btn"><svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="17 1 21 5 17 9"></polyline><path d="M3 11V9a4 4 0 0 1 4-4h14"></path><polyline points="7 23 3 19 7 15"></polyline><path d="M21 13v2a4 4 0 0 1-4 4H3"></path></svg></button>
        </div>
        <div class="p-progress">
          <span class="p-time">1:24</span>
          <div class="p-bar"><div class="p-bar-inner" style="width: 35%"></div></div>
          <span class="p-time">3:45</span>
        </div>
      </div>

      <div class="player-right">
        <button class="p-btn"><svg viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon><path d="M15.54 8.46a5 5 0 0 1 0 7.07"></path></svg></button>
        <div class="v-bar"><div class="v-bar-inner" style="width: 70%"></div></div>
        <button class="p-btn"><svg viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M8 3H5a2 2 0 0 0-2 2v3"></path><path d="M21 8V5a2 2 0 0 0-2-2h-3"></path><path d="M3 16v3a2 2 0 0 0 2 2h3"></path><path d="M16 21h3a2 2 0 0 0 2-2v-3"></path></svg></button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.explore-layout {
  display: flex;
  min-height: calc(100vh - 72px);
  background: #f8f9fa;
  padding-bottom: 90px; /* space for fixed player */
}

/* SIDEBAR */
.explore-sidebar {
  width: 240px;
  background: #fff;
  border-right: 1px solid var(--c-border);
  display: flex;
  flex-direction: column;
  padding: 24px 20px;
}
.sidebar-header {
  margin-bottom: 32px;
}
.sidebar-header h2 {
  font-size: 20px;
  font-weight: 800;
  margin: 0;
  color: #0c1e33;
}
.sidebar-header span {
  font-size: 13px;
  color: var(--c-text-mute);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  font-weight: 600;
}
.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.nav-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 14px;
  border-radius: var(--radius-full);
  color: var(--c-text-soft);
  font-weight: 600;
  font-size: 14.5px;
  text-decoration: none;
  transition: all .2s;
}
.nav-item svg {
  width: 18px;
  height: 18px;
  opacity: 0.7;
}
.nav-item:hover {
  background: var(--c-bg-mute);
  color: var(--c-text);
}
.nav-item.is-active {
  background: #84f2c9;
  color: #0c1e33;
}
.nav-item.is-active svg {
  opacity: 1;
}

.sidebar-bottom {
  margin-top: auto;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.btn-upload {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  background: linear-gradient(135deg, #74e5d6, #84f2c9);
  color: #0c1e33;
  border: none;
  padding: 12px;
  border-radius: var(--radius-full);
  font-weight: 700;
  font-size: 14px;
  cursor: pointer;
  transition: transform .2s, box-shadow .2s;
}
.btn-upload:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(132, 242, 201, 0.4);
}
.settings-link {
  margin-top: 8px;
}

/* MAIN CONTENT */
.explore-main {
  flex: 1;
  padding: 40px;
  max-width: 1200px;
}
.search-container {
  margin-bottom: 24px;
}
.main-search-input {
  width: 100%;
  max-width: 800px;
  background: #fff;
  border: 1px solid var(--c-border);
  border-radius: var(--radius-full);
  padding: 16px 24px;
  font-size: 15px;
  color: var(--c-text);
  box-shadow: 0 2px 10px rgba(0,0,0,0.02);
  outline: none;
  transition: box-shadow .2s, border-color .2s;
}
.main-search-input:focus {
  border-color: #74e5d6;
  box-shadow: 0 4px 16px rgba(116, 229, 214, 0.15);
}
.main-search-input::placeholder {
  color: var(--c-text-mute);
}

.filter-chips {
  display: flex;
  gap: 10px;
  margin-bottom: 40px;
  flex-wrap: wrap;
}
.chip {
  background: #fff;
  border: 1px solid var(--c-border);
  border-radius: var(--radius-full);
  padding: 8px 16px;
  font-size: 13.5px;
  font-weight: 600;
  color: var(--c-text-soft);
  cursor: pointer;
  transition: all .2s;
}
.chip:hover {
  border-color: var(--c-border-strong);
  color: var(--c-text);
}
.chip.is-active {
  background: #74e5d6;
  border-color: #74e5d6;
  color: #0c1e33;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 24px;
}
.section-title {
  font-size: 28px;
  font-weight: 800;
  color: #0c1e33;
  margin: 0 0 8px 0;
}
.section-subtitle {
  color: var(--c-text-soft);
  font-size: 14px;
  margin: 0;
}
.view-toggles {
  display: flex;
  gap: 8px;
}
.toggle-btn {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 1px solid var(--c-border);
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--c-text-mute);
  cursor: pointer;
  transition: all .2s;
}
.toggle-btn:hover {
  color: var(--c-text);
}
.toggle-btn.is-active {
  border-color: #0c1e33;
  color: #0c1e33;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 24px;
}
.exp-card {
  background: #fff;
  border-radius: 20px;
  padding: 16px;
  transition: transform .3s, box-shadow .3s;
}
.exp-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.05);
}
.exp-cover {
  width: 100%;
  aspect-ratio: 1;
  border-radius: 14px;
  margin-bottom: 16px;
  position: relative;
}
.exp-badge {
  position: absolute;
  top: 10px;
  left: 10px;
  background: #84f2c9;
  color: #0c1e33;
  font-size: 10px;
  font-weight: 800;
  padding: 4px 8px;
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
}
.exp-title {
  font-size: 16px;
  font-weight: 700;
  color: #0c1e33;
  margin-bottom: 4px;
}
.exp-artist {
  font-size: 13px;
  color: var(--c-text-soft);
  margin-bottom: 12px;
}
.exp-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.exp-price {
  font-size: 15px;
  font-weight: 800;
  color: #1f6df0;
}
.btn-cart {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: #e8f0fe;
  color: #1f6df0;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all .2s;
}
.btn-cart:hover {
  background: #1f6df0;
  color: #fff;
}

/* FIXED PLAYER */
.fixed-player {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 90px;
  background: #fff;
  border-top: 1px solid var(--c-border);
  box-shadow: 0 -4px 20px rgba(0,0,0,0.03);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 24px;
  z-index: 50;
}
.player-left {
  display: flex;
  align-items: center;
  gap: 16px;
  width: 300px;
}
.p-cover {
  width: 50px;
  height: 50px;
  border-radius: 10px;
}
.p-title {
  font-size: 14.5px;
  font-weight: 700;
  color: #0c1e33;
  margin-bottom: 2px;
}
.p-artist {
  font-size: 12px;
  color: var(--c-text-soft);
}

.player-center {
  flex: 1;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}
.p-controls {
  display: flex;
  align-items: center;
  gap: 20px;
}
.p-btn {
  background: none;
  border: none;
  color: var(--c-text-mute);
  cursor: pointer;
  transition: color .2s;
}
.p-btn:hover {
  color: #0c1e33;
}
.p-btn-play {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: #0c1e33;
  color: #fff;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: transform .2s;
}
.p-btn-play:hover {
  transform: scale(1.05);
}
.p-progress {
  display: flex;
  align-items: center;
  gap: 12px;
  width: 100%;
}
.p-time {
  font-size: 11px;
  color: var(--c-text-mute);
  font-variant-numeric: tabular-nums;
}
.p-bar {
  flex: 1;
  height: 4px;
  background: var(--c-bg-mute);
  border-radius: 2px;
  overflow: hidden;
}
.p-bar-inner {
  height: 100%;
  background: #74e5d6;
  border-radius: 2px;
}

.player-right {
  display: flex;
  align-items: center;
  gap: 16px;
  width: 300px;
  justify-content: flex-end;
}
.v-bar {
  width: 80px;
  height: 4px;
  background: var(--c-bg-mute);
  border-radius: 2px;
}
.v-bar-inner {
  height: 100%;
  background: #0c1e33;
  border-radius: 2px;
}
</style>
