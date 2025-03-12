<script setup lang="ts">
/// <reference types="vite/client" />

import { provide, ref, readonly } from 'vue'
import { RouterView } from 'vue-router'
import Header from './components/AppHeader.vue'
import Sidebar from './components/Sidebar.vue'

import type { Place } from './types/Place.d'

const places = ref<Place[]>([])

const modules = import.meta.glob<{ default: unknown }>('@/assets/articles/*.json', { eager: true })
const requiredKeys: (keyof Place)[] = [
  'name',
  'id',
  'latitude',
  'longitude',
  'preview',
  'previewImage',
  'article',
]
places.value = Object.values(modules)
  .map((mod) => mod.default)
  .filter((el) => {
    return requiredKeys.every((key) => key in (el as object))
  }) as Place[]

provide('places', readonly(places))

//========= Sidebar stuff ============
const isSidebarVisible = ref(true);
const toggleSidebar = () => {
    isSidebarVisible.value = !isSidebarVisible.value;
  };
  
</script>

<template>
  <div class="w-full h-screen flex flex-col">
    <Header />
    <RouterView />
  </div>
  <div class="flex flex-1 overflow-hidden relative">
      <Sidebar
        :is-visible="isSidebarVisible"
        :places="places"
        @toggle-sidebar="toggleSidebar"
      />
    </div>
</template>
