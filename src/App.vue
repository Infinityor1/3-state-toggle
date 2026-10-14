<script setup>
import { ref } from 'vue'
import DepartmentAccordion from './components/DepartmentAccordion.vue'
import SearchBar from './components/SearchBar.vue'

const employees = [
  { name: 'Yi Chern', role: 'Admin', certs: ['Anything Cert 1'] },
  { name: 'Amzar', role: 'Staff', certs: ['Anything Cert 1', 'Anything Cert 2'] },
]
const searchQuery = ref('')
</script>

<template>
  <main>
    <div class="wrapper">
      <div class="modal">
        <SearchBar v-model="searchQuery" />
        <DepartmentAccordion
          department="Technician"
          :employees="employees"
          :search-query="searchQuery"
        />
      </div>
    </div>
  </main>
</template>
