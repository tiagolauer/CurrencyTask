<template>
  <!-- Container for historical prices -->
  <v-container class="historical-price">
    <!-- Title for the historical price section -->
    <h2 class="title pb-6">Coin Price on Specific Date</h2>
    <!-- Card containing form to select coin and date -->
    <v-card class="form-card">
      <v-card-text>
        <!-- Form to select coin and date -->
        <form @submit.prevent="fetchHistoricalPrice" class="form">
          <!-- Select dropdown to choose coin -->
          <div class="selectCoin pb-2">
            <label for="coin">Select Coin:</label>
            <select v-model="selectedCoin" required class="coin-select">
              <option value="bitcoin">Bitcoin (BTC)</option>
              <option value="dacxi">DACXI (DACXI)</option>
              <option value="ethereum">Ethereum (ETH)</option>
              <option value="cosmos">Cosmos (ATOM)</option>
            </select>
          </div>
          <!-- Input field to select date and time -->
          <label class="pb-2" for="datetime">Select Date and Time:</label>
          <input type="datetime-local" v-model="datetime" required class="datetime-input" />
          <!-- Button to trigger fetching of historical price -->
          <div class="button-container">
            <v-btn type="submit" class="price-btn" color="primary">Get Price</v-btn>
          </div>
        </form>
      </v-card-text>
    </v-card>
    <!-- Loading indicator -->
    <div v-if="loading" class="loading">Loading...</div>
    <!-- Display historical price when available -->
    <v-card v-else-if="price !== null" class="price-card pb-2 animated-card">
      <v-card-text>
        <span class="price-label">{{ selectedCoin.toUpperCase() }}/USD: </span>
        <span class="price-value"> ${{ price }}</span>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      // Selected coin
      selectedCoin: 'bitcoin',
      // Selected date and time
      datetime: '',
      // Historical price
      price: null,
      // Loading indicator
      loading: false
    };
  },
  watch: {
    // Reset price when coin selection changes
    selectedCoin() {
      this.price = null;
    }
  },
  methods: {
    // Method to fetch historical price
    async fetchHistoricalPrice() {
      // Set loading to true when fetching starts
      this.loading = true;
      // Reset price before fetching
      this.price = null;
      // Parse selected date
      const date = new Date(this.datetime);
      // Format selected date to required format
      const formattedDate = `${('0' + date.getUTCDate()).slice(-2)}-${('0' + (date.getUTCMonth() + 1)).slice(-2)}-${date.getUTCFullYear()}`;
      try {
        // Fetch historical price from API
        const response = await axios.get(`https://api.coingecko.com/api/v3/coins/${this.selectedCoin}/history?date=${formattedDate}`);
        // Update price with fetched data
        this.price = response.data.market_data.current_price.usd;
      } catch (error) {
        // Log error if fetching fails
        console.error('Error fetching historical price:', error);
      }
      // Set loading to false after fetching completes
      this.loading = false;
    }
  }
};
</script>

<style scoped>
.historical-price {
  max-width: 500px;
  margin: 0 auto;
}

.title {
  text-align: center;
}

.form-card {
  margin-bottom: 20px;
  padding: 20px;
}

.form {
  display: flex;
  flex-direction: column;
}

.coin-select {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 10px;
  width: 100%;
}

.datetime-input {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 10px;
  width: 100%;
}

.button-container {
  display: flex;
  justify-content: center;
  margin-top: 10px;
}

/* Button hover effect */
.price-btn:hover {
  background-color: #0056b3;
}

.price-card {
  margin-top: 10px;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

/* Animation for displaying card */
.animated-card {
  opacity: 1;
  transform: translateY(0);
}

/* Styles for price label and value */
.price-card .price-label,
.price-card .price-value {
  display: inline-block;
  animation: fadeIn 1s forwards;
}

.price-card .price-value {
  animation-delay: 0.5s;
}

/* Loading indicator styles */
.loading {
  font-style: italic;
  color: #888;
  animation: loading-blink 1.5s linear infinite;
}

/* Animation for loading indicator */
@keyframes fadeIn {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

/* Animation for loading indicator blink effect */
@keyframes loading-blink {

  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.5;
  }
}
</style>
