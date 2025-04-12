<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-3xl font-bold mb-6 text-center">Недвижимость в Астане</h1>
    
    <FilterForm @apply-filters="applyFilters" />
    
    <div class="mt-8">
      <p class="text-gray-600 mb-4">Найдено объектов: {{ filteredProperties.length }}</p>
      
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <PropertyCard 
          v-for="property in filteredProperties" 
          :key="property.id" 
          :property="property" 
        />
      </div>
      
      <div v-if="filteredProperties.length === 0" class="text-center py-8">
        <p class="text-gray-500">По вашему запросу ничего не найдено</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import FilterForm from '@/components/FilterForm.vue'
import PropertyCard from '@/components/PropertyCard.vue'

import propertyData from '../../public/properties.json';

export default {
  name: 'App',
  components: {
    FilterForm,
    PropertyCard
  },
  setup() {
    const properties = ref([]);
    const filters = ref({
      areaFrom: null,
      areaTo: null,
      roomsFrom: null,
      roomsTo: null,
      address: ''
    });
    
    const filteredProperties = ref([]);
    
    const applyFilters = (newFilters) => {
      filters.value = newFilters;
      
      filteredProperties.value = properties.value.filter(property => {

        if (filters.value.areaFrom && property.area < filters.value.areaFrom) return false;
        if (filters.value.areaTo && property.area > filters.value.areaTo) return false;
        
        if (filters.value.roomsFrom && property.rooms < filters.value.roomsFrom) return false;
        if (filters.value.roomsTo && property.rooms > filters.value.roomsTo) return false;
        
        if (filters.value.address && !property.address.toLowerCase().includes(filters.value.address.toLowerCase())) return false;
        
        return true;
      });
    };
    
    onMounted(() => {
      properties.value = propertyData;
      filteredProperties.value = propertyData;
    });
    
    return {
      filteredProperties,
      applyFilters
    };
  }
};
</script>