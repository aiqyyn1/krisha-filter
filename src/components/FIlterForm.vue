<template>
  <form @submit.prevent="applyFilters" class="bg-yellow-50 p-6 rounded-lg shadow">
    <h2 class="text-xl font-bold mb-4 text-gray-700">Фильтры поиска</h2>

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
      <div>
        <label class="block mb-1 font-semibold text-gray-700">Площадь (м²)</label>
        <div class="flex gap-2">
          <input
            type="number"
            v-model="filters.areaFrom"
            placeholder="От"
            class="w-full px-3 py-2 border rounded"
          />
          <input
            type="number"
            v-model="filters.areaTo"
            placeholder="До"
            class="w-full px-3 py-2 border rounded"
          />
        </div>
      </div>

      <div>
        <label class="block mb-1 font-semibold text-gray-700">Количество комнат</label>
        <div class="flex gap-2">
          <input
            type="number"
            v-model="filters.roomsFrom"
            placeholder="От"
            class="w-full px-3 py-2 border rounded"
          />
          <input
            type="number"
            v-model="filters.roomsTo"
            placeholder="До"
            class="w-full px-3 py-2 border rounded"
          />
        </div>
      </div>

      <div>
        <label class="block mb-1 font-semibold text-gray-700">Адрес</label>
        <input
          type="text"
          v-model="filters.address"
          placeholder="Введите адрес"
          class="w-full px-3 py-2 border rounded"
        />
      </div>
    </div>

    <div class="mt-6 flex justify-center lg:justify-end gap-2">
      <button
        type="button"
        @click="resetFilters"
        class="px-4 py-2 border border-gray-300 rounded hover:bg-gray-100"
      >
        Сбросить
      </button>
      <button type="submit" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
        Применить
      </button>
    </div>
  </form>
</template>

<script>
import { reactive } from 'vue';

export default {
  name: 'FilterForm',
  emits: ['apply-filters'],
  setup(props, { emit }) {
    const filters = reactive({
      areaFrom: null,
      areaTo: null,
      roomsFrom: null,
      roomsTo: null,
      address: '',
    });

    const applyFilters = () => {
      emit('apply-filters', {
        areaFrom: filters.areaFrom ? Number(filters.areaFrom) : null,
        areaTo: filters.areaTo ? Number(filters.areaTo) : null,
        roomsFrom: filters.roomsFrom ? Number(filters.roomsFrom) : null,
        roomsTo: filters.roomsTo ? Number(filters.roomsTo) : null,
        address: filters.address,
      });
    };

    const resetFilters = () => {
      filters.areaFrom = null;
      filters.areaTo = null;
      filters.roomsFrom = null;
      filters.roomsTo = null;
      filters.address = '';

      emit('apply-filters', {
        areaFrom: null,
        areaTo: null,
        roomsFrom: null,
        roomsTo: null,
        address: '',
      });
    };

    return {
      filters,
      applyFilters,
      resetFilters,
    };
  },
};
</script>
