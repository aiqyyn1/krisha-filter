<template>
  <div class="bg-white rounded-lg shadow overflow-hidden hover:shadow-lg transition-shadow">
    <div class="relative pb-[60%]">
      <img 
        :src="property.image"
        :alt="property.address"
        class="absolute inset-0 w-full h-full object-cover"
      />
      <div class="absolute bottom-0 rounded-sm left-0 bg-blue-500 text-white px-2 py-1 text-sm">
        {{ property.price }} ₸
      </div>
    </div>
    
    <div class="p-4">
      <h3 class="font-bold text-lg mb-2 ">{{ property.address }}</h3>
      
      <div class="flex text-gray-600 mb-2">
        <div class="mr-4">
          <span class="font-medium">{{ property.area }}</span> м²
        </div>
        <div>
          <span class="font-medium">{{ property.rooms }}</span> комн.
        </div>
      </div>
      
      <p class="text-gray-500 text-sm line-clamp-2">{{ property.description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PropertyCard',
  props: {
    property: {
      type: Object,
      required: true
    }
  }
};
</script>