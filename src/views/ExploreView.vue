<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { products, categories, formatVND } from '../data/catalog'
import CustomSelect from '../components/ui/CustomSelect.vue'
import MultiSelect from '../components/ui/MultiSelect.vue'
import ProductCard from '../components/product/ProductCard.vue'

const route = useRoute()
const router = useRouter()

const searchInput = ref('')
const search = ref('')
let debounceTimer = null

watch(searchInput, (newVal) => {
  if (debounceTimer) clearTimeout(debounceTimer)
  debounceTimer = setTimeout(() => {
    search.value = newVal
  }, 500)
})

const activeCats = ref([])
const priceRange = ref('all')
const bpmRange = ref('all')
const sortBy = ref('newest')

const catOptions = computed(() => categories.filter(c => c.id !== 'all').map(c => ({ value: c.id, label: c.label })))

const priceOptions = [
  { value: 'all', label: 'Mọi mức giá' },
  { value: 'under2', label: 'Dưới 2 triệu ₫' },
  { value: '2to5', label: '2 - 5 triệu ₫' },
  { value: 'over5', label: 'Trên 5 triệu ₫' }
]
const bpmOptions = [
  { value: 'all', label: 'Tất cả' },
  { value: 'slow', label: 'Chậm (Dưới 100)' },
  { value: 'medium', label: 'Vừa (100 - 130)' },
  { value: 'fast', label: 'Nhanh (Trên 130)' }
]
const sortOptions = [
  { value: 'newest', label: 'Mới nhất' },
  { value: 'price-asc', label: 'Giá tăng dần' },
  { value: 'price-desc', label: 'Giá giảm dần' }
]

onMounted(() => {
  if (route.query.q) {
    search.value = route.query.q
    searchInput.value = route.query.q
  }
  if (route.query.cats) activeCats.value = route.query.cats.split(',')
  if (route.query.price) priceRange.value = route.query.price
  if (route.query.bpm) bpmRange.value = route.query.bpm
  if (route.query.sort) sortBy.value = route.query.sort
})

watch([search, activeCats, priceRange, bpmRange, sortBy], () => {
  router.replace({
    query: {
      q: search.value || undefined,
      cats: activeCats.value.length > 0 ? activeCats.value.join(',') : undefined,
      price: priceRange.value !== 'all' ? priceRange.value : undefined,
      bpm: bpmRange.value !== 'all' ? bpmRange.value : undefined,
      sort: sortBy.value !== 'newest' ? sortBy.value : undefined
    }
  })
})

function resetFilters() {
  searchInput.value = ''
  search.value = ''
  activeCats.value = []
  priceRange.value = 'all'
  bpmRange.value = 'all'
  sortBy.value = 'newest'
}

const activeCatLabels = computed(() => {
  return activeCats.value.map(cat => {
    const found = categories.find(c => c.id === cat)
    return found ? found.label : cat
  }).join(', ')
})

const exploreProducts = computed(() => {
  const q = search.value.trim().toLowerCase()
  let result = products.filter(p => {
    // Thể loại filter
    const matchCat = activeCats.value.length === 0 || activeCats.value.every(cat => {
      return p.category === cat || (p.tags && p.tags.includes(cat)) || p.category.toLowerCase().includes(cat.toLowerCase())
    })

    const matchQ = !q || p.title.toLowerCase().includes(q) || p.artist.toLowerCase().includes(q)
    
    let matchPrice = true
    if (priceRange.value === 'under2') matchPrice = p.basePrice < 2000000
    if (priceRange.value === '2to5') matchPrice = p.basePrice >= 2000000 && p.basePrice <= 5000000
    if (priceRange.value === 'over5') matchPrice = p.basePrice > 5000000
    
    let matchBpm = true
    if (bpmRange.value === 'slow') matchBpm = p.bpm < 100
    if (bpmRange.value === 'medium') matchBpm = p.bpm >= 100 && p.bpm <= 130
    if (bpmRange.value === 'fast') matchBpm = p.bpm > 130
    
    return matchCat && matchQ && matchPrice && matchBpm
  })

  if (sortBy.value === 'price-asc') {
    result.sort((a, b) => a.basePrice - b.basePrice)
  } else if (sortBy.value === 'price-desc') {
    result.sort((a, b) => b.basePrice - a.basePrice)
  }

  return result
})
</script>

<template>
  <div class="explore-container">
    <!-- Breadcrumb -->
    <div class="container crumbs-header">
      <RouterLink to="/">Trang chủ</RouterLink>
      <span>›</span>
      <span class="muted">Marketplace</span>
      <span v-if="activeCats.length > 0">›</span>
      <span v-if="activeCats.length > 0" class="muted">{{ activeCatLabels }}</span>
    </div>

    <div class="explore-layout container">
      <div class="explore-main-wrapper">
        <!-- GLOBAL SEARCH BAR -->
        <div class="global-search-wrapper">
          <svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="search-icon"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
          <input v-model="searchInput" type="text" placeholder="Tìm kiếm tên bài hát, tác giả, v.v..." class="global-search-input" />
        </div>

        <div class="explore-body">
          <!-- ADVANCED FILTERS -->
          <aside class="explore-sidebar">
            <div class="advanced-filters">
              <h3 class="filter-title">Lọc & Phân loại</h3>
              
              <div class="filter-group">
                <label>Thể loại & Tags</label>
                <MultiSelect v-model="activeCats" :options="catOptions" />
              </div>

              <div class="filter-group">
                <label>Khoảng giá</label>
                <CustomSelect v-model="priceRange" :options="priceOptions" />
              </div>

              <div class="filter-group">
                <label>Nhịp độ (BPM)</label>
                <CustomSelect v-model="bpmRange" :options="bpmOptions" />
              </div>

              <div class="filter-group">
                <label>Sắp xếp</label>
                <CustomSelect v-model="sortBy" :options="sortOptions" />
              </div>
            </div>
          </aside>

          <!-- MAIN CONTENT -->
          <main class="explore-main">
            <div class="section-header">
              <div>
                <h1 class="section-title">Marketplace</h1>
                <p class="section-subtitle">Hơn 50,000 bản quyền âm nhạc chất lượng cao dành cho dự án của bạn.</p>
              </div>
              <div class="result-count">
                Tìm thấy <strong>{{ exploreProducts.length }}</strong> kết quả
              </div>
            </div>

            <div v-if="exploreProducts.length === 0" class="empty-state">
              <div class="empty-illu">🎧</div>
              <h3>Không tìm thấy kết quả</h3>
              <p>Rất tiếc, không có bài hát nào phù hợp với bộ lọc hiện tại của bạn.</p>
              <button @click="resetFilters" class="btn btn-primary">Xoá bộ lọc</button>
            </div>

            <div v-else class="product-grid">
              <div v-for="p in exploreProducts" :key="p.id" class="grid-item">
                <ProductCard :product="p" />
              </div>
            </div>
          </main>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.explore-container {
  display: flex;
  flex-direction: column;
  min-height: calc(100vh - 72px);
  background: #f8f9fa;
}
.crumbs-header {
  padding: 24px 20px 8px;
  display: flex; gap: 8px; align-items: center; font-size: 13px; color: var(--c-text-soft);
}
.crumbs-header a { color: var(--c-blue-700); text-decoration: none; }
.crumbs-header .muted { color: var(--c-text-mute); }

.explore-layout {
  display: flex;
  flex: 1;
  padding: 16px 20px 60px;
}
.explore-main-wrapper {
  display: flex;
  flex-direction: column;
  width: 100%;
}

/* Global Search Input */
.global-search-wrapper {
  display: flex;
  align-items: center;
  background: #fff;
  border: 1px solid var(--c-border);
  border-radius: var(--radius-lg);
  padding: 16px 24px;
  margin-bottom: 32px;
  box-shadow: var(--shadow-sm);
  transition: all .2s;
}
.global-search-wrapper:focus-within {
  border-color: #74e5d6;
  box-shadow: 0 4px 16px rgba(116, 229, 214, 0.15);
}
.search-icon {
  color: var(--c-text-mute);
  margin-right: 14px;
}
.global-search-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: 16px;
  color: var(--c-text);
  background: transparent;
}
.global-search-input::placeholder {
  color: var(--c-text-mute);
}

.explore-body {
  display: flex;
  gap: 32px;
  align-items: flex-start;
}

/* SIDEBAR */
.explore-sidebar {
  width: 250px;
  flex-shrink: 0;
  position: sticky;
  top: 100px;
}
.advanced-filters {
  background: #fff;
  border: 1px solid var(--c-border);
  border-radius: var(--radius-lg);
  padding: 20px;
  box-shadow: var(--shadow-sm);
}
.filter-title {
  font-size: 15px;
  font-weight: 800;
  margin: 0 0 16px 0;
  color: #0c1e33;
}
.filter-group {
  margin-bottom: 20px;
}
.filter-group:last-child {
  margin-bottom: 0;
}
.filter-group label {
  display: block;
  font-size: 12px;
  font-weight: 700;
  color: var(--c-text-soft);
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

/* MAIN CONTENT */
.explore-main {
  flex: 1;
  min-width: 0;
}
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 24px;
  padding-bottom: 16px;
  border-bottom: 1px solid var(--c-border);
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
.result-count {
  font-size: 13.5px;
  color: var(--c-text-soft);
}
.result-count strong {
  color: #0c1e33;
}

.empty-state {
  background: #fff;
  border: 1px dashed var(--c-border-strong);
  border-radius: var(--radius-xl);
  padding: 80px 24px;
  text-align: center;
}
.empty-illu { font-size: 48px; margin-bottom: 14px; }
.empty-state h3 { margin: 0 0 6px; font-size: 22px; color: #0c1e33; }
.empty-state p { color: var(--c-text-soft); margin: 0 0 22px; }

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 24px;
}
</style>
