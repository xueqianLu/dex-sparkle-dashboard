<template>
  <div class="flex items-center">
    <button v-if="!isConnected" @click="connectWallet" class="btn-primary">
      Connect Wallet
    </button>
    <div v-else class="flex items-center space-x-2">
      <span class="text-sm text-gray-400">{{ shortAddress }}</span>
      <button @click="disconnectWallet" class="text-sm text-red-500">
        Disconnect
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import Web3 from 'web3'

export default {
  name: 'WalletConnect',
  setup() {
    const isConnected = ref(false)
    const account = ref('')
    
    const shortAddress = computed(() => {
      if (!account.value) return ''
      return `${account.value.slice(0, 6)}...${account.value.slice(-4)}`
    })

    const connectWallet = async () => {
      try {
        if (typeof window.ethereum !== 'undefined') {
          const accounts = await window.ethereum.request({ 
            method: 'eth_requestAccounts' 
          })
          account.value = accounts[0]
          isConnected.value = true
        } else {
          alert('Please install MetaMask!')
        }
      } catch (error) {
        console.error('Error connecting wallet:', error)
      }
    }

    const disconnectWallet = () => {
      account.value = ''
      isConnected.value = false
    }

    return {
      isConnected,
      shortAddress,
      connectWallet,
      disconnectWallet
    }
  }
}
</script>