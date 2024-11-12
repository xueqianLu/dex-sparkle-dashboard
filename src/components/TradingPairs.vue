<template>
  <div class="card">
    <h2 class="text-lg font-semibold mb-4">Trading Pairs</h2>
    <div class="space-y-2">
      <div
        v-for="pair in pairs"
        :key="pair.address"
        @click="selectPair(pair)"
        :class="[
          'p-3 rounded cursor-pointer hover:bg-gray-700',
          selectedPair?.address === pair.address ? 'bg-gray-700' : ''
        ]"
      >
        <div class="flex justify-between items-center">
          <span>{{ pair.name }}</span>
          <span :class="[
            pair.priceChange >= 0 ? 'text-green-500' : 'text-red-500'
          ]">
            {{ pair.price }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import axios from 'axios'

export default {
  name: 'TradingPairs',
  setup(props, { emit }) {
    const pairs = ref([])
    const selectedPair = ref(null)

    const fetchPairs = async () => {
      try {
        const response = await axios.get('http://52.221.177.10:38082/api/v1/pair-list')
        pairs.value = response.data
        if (pairs.value.length > 0) {
          selectPair(pairs.value[0])
        }
      } catch (error) {
        console.error('Error fetching pairs:', error)
      }
    }

    const selectPair = (pair) => {
      selectedPair.value = pair
      emit('pair-selected', pair)
    }

    onMounted(() => {
      fetchPairs()
    })

    return {
      pairs,
      selectedPair,
      selectPair
    }
  }
}
</script>