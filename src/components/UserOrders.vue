<template>
  <div class="space-y-6">
    <div class="card">
      <h2 class="text-lg font-semibold mb-4">Open Orders</h2>
      <div class="overflow-x-auto">
        <table class="w-full">
          <thead>
            <tr class="text-left text-sm text-gray-400">
              <th class="pb-4">Pair</th>
              <th class="pb-4">Side</th>
              <th class="pb-4">Price</th>
              <th class="pb-4">Amount</th>
              <th class="pb-4">Total</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="order in openOrders" :key="order.id" class="text-sm">
              <td class="py-2">{{ order.pair }}</td>
              <td class="py-2" :class="order.side === 0 ? 'text-green-500' : 'text-red-500'">
                {{ order.side === 0 ? 'Buy' : 'Sell' }}
              </td>
              <td class="py-2">{{ order.price }}</td>
              <td class="py-2">{{ order.amount }}</td>
              <td class="py-2">{{ order.price * order.amount }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div class="card">
      <h2 class="text-lg font-semibold mb-4">Order History</h2>
      <div class="overflow-x-auto">
        <table class="w-full">
          <thead>
            <tr class="text-left text-sm text-gray-400">
              <th class="pb-4">Date</th>
              <th class="pb-4">Pair</th>
              <th class="pb-4">Side</th>
              <th class="pb-4">Price</th>
              <th class="pb-4">Amount</th>
              <th class="pb-4">Status</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="order in orderHistory" :key="order.id" class="text-sm">
              <td class="py-2">{{ formatDate(order.date) }}</td>
              <td class="py-2">{{ order.pair }}</td>
              <td class="py-2" :class="order.side === 0 ? 'text-green-500' : 'text-red-500'">
                {{ order.side === 0 ? 'Buy' : 'Sell' }}
              </td>
              <td class="py-2">{{ order.price }}</td>
              <td class="py-2">{{ order.amount }}</td>
              <td class="py-2">{{ order.status }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import axios from 'axios'

export default {
  name: 'UserOrders',
  setup() {
    const openOrders = ref([])
    const orderHistory = ref([])

    const fetchOrders = async () => {
      try {
        const [openResponse, historyResponse] = await Promise.all([
          axios.post('http://52.221.177.10:38082/api/v1/user-open-orders'),
          axios.post('http://52.221.177.10:38082/api/v1/user-order-history')
        ])
        openOrders.value = openResponse.data
        orderHistory.value = historyResponse.data
      } catch (error) {
        console.error('Error fetching orders:', error)
      }
    }

    const formatDate = (date) => {
      return new Date(date).toLocaleString()
    }

    onMounted(() => {
      fetchOrders()
    })

    return {
      openOrders,
      orderHistory,
      formatDate
    }
  }
}
</script>