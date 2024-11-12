<template>
  <div class="card">
    <div class="flex space-x-4 mb-4">
      <button 
        :class="[
          'flex-1 py-2 rounded-lg',
          isBuy ? 'bg-green-500' : 'bg-gray-700'
        ]"
        @click="isBuy = true"
      >
        Buy
      </button>
      <button
        :class="[
          'flex-1 py-2 rounded-lg',
          !isBuy ? 'bg-red-500' : 'bg-gray-700'
        ]"
        @click="isBuy = false"
      >
        Sell
      </button>
    </div>
    
    <div class="space-y-4">
      <div>
        <label class="block text-sm mb-2">Price</label>
        <input
          v-model="price"
          type="number"
          class="w-full bg-gray-700 rounded p-2"
        />
      </div>
      
      <div>
        <label class="block text-sm mb-2">Amount</label>
        <input
          v-model="amount"
          type="number"
          class="w-full bg-gray-700 rounded p-2"
        />
      </div>
      
      <div class="text-right text-sm">
        <div>Total: {{ total }}</div>
      </div>
      
      <button
        @click="submitOrder"
        :class="[
          'w-full py-3 rounded-lg',
          isBuy ? 'bg-green-500' : 'bg-red-500'
        ]"
      >
        {{ isBuy ? 'Buy' : 'Sell' }}
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import axios from 'axios'

export default {
  name: 'TradeForm',
  props: {
    pair: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const isBuy = ref(true)
    const price = ref('')
    const amount = ref('')
    
    const total = computed(() => {
      return (price.value * amount.value).toFixed(8)
    })
    
    const submitOrder = async () => {
      try {
        await axios.post('http://52.221.177.10:38082/api/v1/user-make-order', {
          pair: props.pair.address,
          price: price.value,
          amount: amount.value,
          side: isBuy.value ? 0 : 1
        })
        price.value = ''
        amount.value = ''
      } catch (error) {
        console.error('Error submitting order:', error)
      }
    }
    
    return {
      isBuy,
      price,
      amount,
      total,
      submitOrder
    }
  }
}
</script>