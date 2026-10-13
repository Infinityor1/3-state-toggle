<script setup>
import DepartmentAccordion from './components/DepartmentAccordion.vue'
</script>

<template>
  <main>
    <div class="wrapper">
      <div class="modal">
        <DepartmentAccordion department="Technician" :employees="[{name:'Yi Chern',role:'Admin',certs:['Heavy Machinery Certification']}, {name:'Amzar',role:'Staff',certs:['Anything Cert 1','Anything Cert 2']}]" />
      </div>
    </div>
  </main>
</template>
