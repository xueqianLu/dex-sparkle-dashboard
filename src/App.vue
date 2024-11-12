<template>
  <div class="min-h-screen bg-background p-6">
    <header class="flex justify-between items-center mb-8">
      <h1 class="text-2xl font-bold">DEX Exchange</h1>
      <WalletConnect />
    </header>

    <div class="grid grid-cols-12 gap-6">
      <div class="col-span-3">
        <TradingPairs @pair-selected="handlePairSelected" />
      </div>
      
      <div class="col-span-6">
        <div class="space-y-6">
          <div class="card">
            <trading-view-widget
              :symbol="selectedPair?.symbol"
              :theme="'Dark'"
              :width="'100%'"
              :height="400"
            />
          </div>
          <TradeForm :pair="selectedPair" />
        </div>
      </div>
      
      <div class="col-span-3">
        <OrderBook :pair="selectedPair" />
      </div>
    </div>

    <div class="mt-8">
      <UserOrders />
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import WalletConnect from './components/WalletConnect.vue'
import TradingPairs from './components/TradingPairs.vue'
import OrderBook from './components/OrderBook.vue'
import TradeForm from './components/TradeForm.vue'
import UserOrders from './components/UserOrders.vue'
import { TradingViewWidget } from 'vue-trading-view'

export default {
  name: 'App',
  components: {
    WalletConnect,
    TradingPairs,
    OrderBook,
    TradeForm,
    UserOrders,
    TradingViewWidget
  },
  setup() {
    const selectedPair = ref(null)

    const handlePairSelected = (pair) => {
      selectedPair.value = pair
    }

    return {
      selectedPair,
      handlePairSelected
    }
  }
}
</script>