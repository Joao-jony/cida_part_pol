<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Mail, Phone, Search, Menu, X, ChevronDown } from 'lucide-vue-next'
import logoPrincipal from '../assets/logo-principal.jpg'
import logoSecundario from '../assets/logo-secundario.png'

const menu = [
  { label: 'Início', href: '#' },
  {
    label: 'Sobre TCA',
    children: ['O Tribunal', 'Organização', 'História'],
  },
  {
    label: 'Fiscalização',
    children: ['Fiscalização Prévia', 'Fiscalização Sucessiva', 'Fiscalização Concomitante'],
  },
  {
    label: 'Actos TCA',
    children: ['Acórdãos', 'Despachos', 'Resoluções'],
  },
  {
    label: 'Transparência',
    children: ['Relatórios', 'Plano e orçamento', 'Contratação pública'],
  },
  {
    label: 'Biblioteca',
    children: ['Publicações', 'Legislação', 'Documentos'],
  },
  { label: 'Cooperação Institucional', href: '#' },
  { label: 'Eventos', href: '#' },
  {
    label: 'Área Restrita',
    right: true,
    children: ['Entrar', 'Portal interno'],
  },
]

const lang = ref('PT')
const openIndex = ref(null)
const mobileOpen = ref(false)
const searchOpen = ref(false)
const query = ref('')
const searchInput = ref(null)

function toggleDropdown(i) {
  openIndex.value = openIndex.value === i ? null : i
}

function setLang(l) {
  lang.value = l
  document.documentElement.lang = l.toLowerCase()
}

function toggleSearch() {
  searchOpen.value = !searchOpen.value
  if (searchOpen.value) setTimeout(() => searchInput.value?.focus(), 0)
}

function submitSearch() {
  const q = query.value.trim()
  if (q) window.location.href = `?s=${encodeURIComponent(q)}`
}

function closeAll() {
  openIndex.value = null
}

onMounted(() => document.addEventListener('click', closeAll))
onBeforeUnmount(() => document.removeEventListener('click', closeAll))
</script>

<template>
  <header class="site-header">
    <svg class="deco deco-left" viewBox="0 0 280 188" preserveAspectRatio="xMinYMin slice" aria-hidden="true" focusable="false">
      <polygon fill="#cc0a12" points="0,15 272,188 214,188 0,51" />
      <polygon fill="#ffbf0d" points="0,62 198,188 119,188 0,112" />
      <polygon fill="#000" points="0,112 119,188 0,188" />
    </svg>
    <svg class="deco deco-right" viewBox="0 108 203 140" preserveAspectRatio="xMaxYMax slice" aria-hidden="true" focusable="false">
      <polygon fill="#c90710" points="22,247 149,0 202,0 202,110 105,174" />
      <polygon fill="#8a0405" points="202,29 118,158 202,47" />
      <polygon fill="#fcbb06" points="202,47 54,247 170,162 184,139 202,119" />
      <polygon fill="#000" points="202,132 107,228 202,172" />
    </svg>
    <div class="header-inner">
      <a class="brand-large" href="#" aria-label="Tribunal de Contas de Angola">
        <img :src="logoPrincipal" alt="Tribunal de Contas de Angola" />
      </a>

      <div class="header-content">
        <div class="topbar">
          <div class="contact-items">
            <a href="mailto:info.nosporangola@gmail.com"><Mail :size="14" /><span>info.nosporangola@gmail.com</span></a>
            <span class="separator"></span>
            <a href="tel:+244951366673"><Phone :size="14" /><span>+244 951 366 673</span></a>
            <span class="separator"></span>
          </div>
          <a class="brand-small" href="#" aria-label="Página inicial">
            <img :src="logoSecundario" alt="Logótipo do Tribunal de Contas" />
          </a>
        </div>

        <nav class="main-nav" aria-label="Navegação principal" @click.stop>
          <button
            class="mobile-toggle"
            :aria-label="mobileOpen ? 'Fechar menu' : 'Abrir menu'"
            :aria-expanded="mobileOpen"
            @click="mobileOpen = !mobileOpen"
          >
            <X v-if="mobileOpen" :size="22" />
            <Menu v-else :size="22" />
          </button>

          <ul class="nav-list" :class="{ 'mobile-open': mobileOpen }">
            <li
              v-for="(item, i) in menu"
              :key="item.label"
              :class="{ 'has-dropdown': item.children, open: openIndex === i }"
            >
              <a v-if="!item.children" :href="item.href">{{ item.label }}</a>
              <template v-else>
                <button
                  class="nav-trigger"
                  :aria-expanded="openIndex === i"
                  @click.stop="toggleDropdown(i)"
                >
                  {{ item.label }} <ChevronDown :size="12" />
                </button>
                <ul class="dropdown" :class="{ 'dropdown-right': item.right }">
                  <li v-for="child in item.children" :key="child">
                    <a href="#">{{ child }}</a>
                  </li>
                </ul>
              </template>
            </li>
          </ul>

          <button class="search-toggle" aria-label="Abrir pesquisa" @click="toggleSearch">
            <Search :size="24" />
          </button>
          <form
            class="search-form"
            :class="{ open: searchOpen }"
            role="search"
            @submit.prevent="submitSearch"
          >
            <input
              ref="searchInput"
              v-model="query"
              type="search"
              placeholder="Pesquisar..."
              aria-label="Pesquisar no site"
            />
            <button type="submit" aria-label="Pesquisar"><Search :size="16" /></button>
          </form>
        </nav>
      </div>
    </div>
  </header>
</template>

<style scoped>
.site-header{position:relative;width:100%;height:188px;background:#fff;overflow:hidden}
.deco{position:absolute;top:0;height:188px;pointer-events:none}
.deco-left{left:0;width:280px}
.deco-right{right:0;width:272px}
.header-inner{position:relative;max-width:1500px;height:188px;margin:0 auto;display:flex;align-items:stretch;gap:40px}
.brand-large{flex:0 0 241px;height:179px;margin-top:5px;background:transparent;display:flex;align-items:center;justify-content:center}
.brand-large img{width:100%;height:100%;object-fit:contain;mix-blend-mode:multiply}
.header-content{flex:1;min-width:0}
.topbar{height:125px;display:flex;align-items:flex-start;padding-top:28px;gap:24px}
.contact-items{display:flex;align-items:center;gap:18px;padding-top:20px;white-space:nowrap}
.contact-items a{display:flex;align-items:center;gap:14px;color:#777;text-decoration:none;font-size:14px}
.contact-items svg{color:var(--navy)}
.separator{height:8px;width:20px;background:var(--navy);display:inline-block}
.languages{display:flex;flex-direction:column;line-height:21px}
.languages button{border:0;background:none;padding:0;color:#777;font-size:14px;cursor:pointer;text-align:left}
.languages button.active{font-weight:600}
.brand-small{margin-left:auto;margin-right:92px;display:block;width:82px;height:82px}
.brand-small img{width:100%;height:100%;object-fit:contain;mix-blend-mode:multiply}
.main-nav{height:63px;border-top:1px solid var(--line);display:flex;align-items:center;position:relative}
.nav-list{display:flex;align-items:center;gap:27px;list-style:none;margin:0;padding:0 18px 0 6px;flex:1;min-width:0}
.nav-list>li{position:relative;white-space:nowrap}
.nav-list a,.nav-trigger{font-size:13px;color:var(--text);text-decoration:none;background:none;border:0;padding:0;cursor:pointer;font-family:inherit}
.nav-trigger{display:flex;align-items:center;gap:10px}
.nav-list a:hover,.nav-trigger:hover{color:#a67a1d}
.dropdown{display:none;position:absolute;top:calc(100% + 17px);left:-12px;min-width:190px;background:#fff;box-shadow:0 5px 18px #0002;border-top:3px solid var(--gold);padding:8px 0;list-style:none;margin:0;z-index:10}
.dropdown li a{display:block;padding:10px 14px;font-size:13px}
.has-dropdown.open>.dropdown{display:block}
.dropdown-right{left:auto;right:0}
.search-toggle{height:49px;width:49px;border:0;background:#f1f1f1;border-radius:3px;color:#333;cursor:pointer;flex:0 0 auto;display:flex;align-items:center;justify-content:center}
.search-form{display:none;position:absolute;right:0;top:63px;background:white;padding:10px;box-shadow:0 4px 12px #0002;z-index:12}
.search-form.open{display:flex}
.search-form input{border:1px solid #ccc;padding:10px;min-width:220px}
.search-form button{border:0;background:var(--navy);color:white;padding:0 14px;cursor:pointer}
.mobile-toggle{display:none}

@media(max-width:1500px){
  .deco-left{width:200px}
  .deco-right{width:200px}
}
@media(max-width:1200px){
  .deco{display:none}
  .header-inner{padding:0 18px;gap:24px}
  .brand-large{flex-basis:200px}
  .nav-list{gap:16px}
  .nav-list a,.nav-trigger{font-size:12px}
  .brand-small{margin-right:20px}
}
@media(max-width:900px){
  .site-header{height:auto;overflow:visible}
  .deco{display:none}
  .header-inner{gap:0;padding:0;display:block;height:auto}
  .brand-large{display:none}
  .topbar{height:92px;padding:12px 18px;align-items:center}
  .contact-items{padding-top:0;gap:12px}
  .contact-items a{font-size:12px;gap:8px}
  .separator{width:12px}
  .brand-small{width:60px;height:60px;margin-right:0}
  .languages{font-size:12px;line-height:17px}
  .main-nav{padding:0 14px;height:58px}
  .mobile-toggle{display:block;border:0;background:none;color:var(--navy);cursor:pointer}
  .nav-list{display:none;position:absolute;top:58px;left:0;right:0;background:white;box-shadow:0 5px 14px #0002;z-index:20;padding:10px 18px;align-items:stretch;gap:0;flex-direction:column}
  .nav-list.mobile-open{display:flex}
  .nav-list>li{white-space:normal;border-bottom:1px solid #eee}
  .nav-list>li>a,.nav-trigger{display:flex;width:100%;padding:13px 0;justify-content:space-between;font-size:14px}
  .dropdown{position:static;box-shadow:none;border-top:0;padding:0 0 6px 12px}
  .dropdown li a{padding:9px}
  .search-toggle{margin-left:auto}
  .search-form{right:14px}
}
@media(max-width:560px){
  .topbar{gap:12px}
  .contact-items{flex-wrap:wrap;gap:8px}
  .contact-items a span{font-size:11px}
  .contact-items a:first-child{max-width:150px}
  .contact-items a:first-child span{overflow:hidden;text-overflow:ellipsis}
  .contact-items .separator{display:none}
  .brand-small{width:48px;height:48px}
  .languages{font-size:11px}
}
</style>
