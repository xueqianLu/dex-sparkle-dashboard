<template>
  <div class="card">
    <h2 class="text-lg font-semibold mb-4">Order Book</h2>
    <div class="space-y-4">
      <div class="space-y-1">
        <div class="text-red-500 space-y-1">
          <div v-for="order in sellOrders" :key="order.id" 
               class="flex justify-between text-sm">
            <span>{{ order.price }}</span>
            <span>{{ order.amount }}</span>
          </div>
        </div>
        <div class="text-center py-2 text-xl font-bold">
          {{ currentPrice }}
        </div>
        <div class="text-green-500 space-y-1">
          <div v-for="order in buyOrders" :key="order.id" 
               class="flex justify-between text-sm">
            <span>{{ order.price }}</span>
            <span>{{ order.amount }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, watch } from 'vue'
import axios from 'axios'

export default {
  name: 'OrderBook',
  props: {
    pair: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const buyOrders = ref([])
    const sellOrders = ref([])
    const currentPrice = ref('0.00')

    const fetchOrders = async () => {
      try {
        const response = await axios.get(
          `http://52.221.177.10:38082/api/v1/pair-order-list/${props.pair.address}`
        )
        buyOrders.value = response.data.buyOrders
        sellOrders.value = response.data.sellOrders
        currentPrice.value = response.data.currentPrice
      } catch (error) {
        console.error('Error fetching orders:', error)
      }
    }

    watch(() => props.pair, () => {
      fetchOrders()
    })

    return {
      buyOrders,
      sellOrders,
      currentPrice
    }
  }
}
</script>