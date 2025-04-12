<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-3xl font-bold mb-6 text-center">Недвижимость в Астане</h1>
    
    <FilterForm :loading="loading" @apply-filters="handleApplyFilters" />

    <div class="mt-8">
      <p class="text-gray-600 mb-4" v-if="!loading">
        Найдено объектов: {{ filteredProperties.length }}
      </p>
      

      <div v-if="loading" class="text-center py-8">
        <p class="text-gray-500">Загрузка данных...</p>
      </div>
      
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <PropertyCard 
          v-for="property in paginatedProperties" 
          :key="property.id" 
          :property="property" 
        />
      </div>
      
      <div v-if="!loading && filteredProperties.length === 0" class="text-center py-8">
        <p class="text-gray-500">По вашему запросу ничего не найдено</p>
      </div>
      

      <div v-if="!loading && totalPages > 1" class="flex justify-center mt-6">
        <button 
          @click="goToPage(currentPage - 1)" 
          :disabled="currentPage === 1"
          class="px-3 py-1 border rounded mr-2 disabled:opacity-50">
          Prev
        </button>
        
        <template v-for="n in totalPages" :key="n">
          <button 
            @click="goToPage(n)" 
            :class="[
              'px-3 py-1 rounded mx-1 border',
              currentPage === n ? 'bg-blue-500 text-white border-blue-500' : 'text-gray-700'
            ]">
            {{ n }}
          </button>
        </template>
        
        <button 
          @click="goToPage(currentPage + 1)" 
          :disabled="currentPage === totalPages"
          class="px-3 py-1 border rounded ml-2 disabled:opacity-50">
          Next
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue';
import FilterForm from '@/components/FilterForm.vue';
import PropertyCard from '@/components/PropertyCard.vue';

export default {
  name: 'App',
  components: {
    FilterForm,
    PropertyCard
  },
  setup() {
    const properties = ref([]);          
    const filteredProperties = ref([]);       
    const loading = ref(true);
    const currentPage = ref(1);
    const itemsPerPage = 3;                 


    const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));


    const handleApplyFilters = async (newFilters) => {
      loading.value = true;
      await delay(500);
      
 
      filteredProperties.value = properties.value.filter(property => {
        if (newFilters.areaFrom && property.area < newFilters.areaFrom) return false;
        if (newFilters.areaTo && property.area > newFilters.areaTo) return false;
        if (newFilters.roomsFrom && property.rooms < newFilters.roomsFrom) return false;
        if (newFilters.roomsTo && property.rooms > newFilters.roomsTo) return false;
        if (newFilters.address && !property.address.toLowerCase().includes(newFilters.address.toLowerCase())) return false;
        return true;
      });
      
 
      currentPage.value = 1;
      loading.value = false;
    };
    

    onMounted(async () => {
      try {
        const res = await fetch('/properties.json');
        const data = await res.json();
        properties.value = data;
        filteredProperties.value = data;
      } catch (error) {
        console.error('Ошибка загрузки данных:', error);
      } finally {
        loading.value = false;
      }
    });
    
    const totalPages = computed(() => {
      return Math.ceil(filteredProperties.value.length / itemsPerPage);
    });
    
    const paginatedProperties = computed(() => {
      const start = (currentPage.value - 1) * itemsPerPage;
      return filteredProperties.value.slice(start, start + itemsPerPage);
    });
    
    const goToPage = (n) => {
      if (n < 1 || n > totalPages.value) return;
      currentPage.value = n;
    };
    
    return {
      filteredProperties,
      paginatedProperties,
      handleApplyFilters,
      loading,
      currentPage,
      totalPages,
      goToPage
    };
  }
};
</script>
