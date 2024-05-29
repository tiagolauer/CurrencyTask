<template>
  <!-- Container for current prices -->
  <v-container class="current-price">
    <!-- Heading for current prices section -->
    <h2 class="pb-6">Current Coin Prices</h2>
    <!-- Row to contain price cards -->
    <v-row justify="center">
      <!-- Loop through prices to display each coin's price -->
      <v-col v-for="(price, coin) in prices" :key="coin" cols="12" sm="6" md="4" class="price-col">
        <!-- Display price card when data is loaded -->
        <v-card class="price-card" v-if="!loading">
          <v-card-text class="price-text" style="color: #d1e000;">{{ coin.toUpperCase() }}/USD: ${{ price
            }}</v-card-text>
        </v-card>
        <!-- Display skeleton loader while data is loading -->
        <v-skeleton-loader class="skeleton-loader" v-else></v-skeleton-loader>
      </v-col>
    </v-row>
    <!-- Error message -->
    <div v-if="error" class="error-message">
      <p>{{ error }}</p>
      <button @click="retryFetch">Retry</button>
    </div>
    <v-divider></v-divider>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      // Object to store prices of different coins
      prices: {
        bitcoin: null,
        dacxi: null,
        ethereum: null,
        cosmos: null
      },
      // Loading indicator
      loading: true,
      // Error message
      error: null
    };
  },
  mounted() {
    // Fetch initial prices and set interval to update every minute
    this.fetchCurrentPrices();
    setInterval(this.fetchCurrentPrices, 60000); // Update every minute
  },
  methods: {
    // Method to fetch current prices from the API
    async fetchCurrentPrices() {
      try {
        // Reset error
        this.error = null;
        // Fetch prices from the API
        const response = await axios.get('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,dacxi,ethereum,cosmos&vs_currencies=usd');
        // Update prices in the data object
        this.prices.bitcoin = response.data.bitcoin.usd;
        this.prices.dacxi = response.data.dacxi.usd;
        this.prices.ethereum = response.data.ethereum.usd;
        this.prices.cosmos = response.data.cosmos.usd;
        // Set loading to false when data is successfully fetched
        this.loading = false;
      } catch (error) {
        // Log error if fetching fails
        console.error('Error fetching current prices:', error);
        // Set loading to false even if fetching fails
        this.loading = false;
        // Set error message
        this.error = 'Error fetching current prices. Please retry.';
      }
    },
    // Method to retry fetching prices
    retryFetch() {
      this.loading = true;
      this.fetchCurrentPrices();
    }
  }
};
</script>

<style scoped>
h2 {
  text-align: center;
  color: #d1e000;
}

.current-price {
  max-width: 90%;
  margin: 0 auto;
}

.price-col {
  margin-bottom: 20px;
}

.price-card {
  padding: 10px;
  text-align: center;
}

.price-text {
  font-weight: bold;
}

.skeleton-loader {
  height: 50px;
}

.error-message {
  text-align: center;
  margin-top: 20px;
}
</style>
