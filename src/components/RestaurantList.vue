<template>
  <div class="restaurant-list-container">
    <div class="header">
      <h1>Restaurant List</h1>
    </div>

    <!-- Restaurant List View -->
    <div v-if="!selectedRestaurant" class="restaurant-list">
      <div
        class="restaurant"
        v-for="restaurant in restaurants"
        :key="restaurant.id"
        @click="showRestaurantDetails(restaurant)"
        :style="{ backgroundColor: restaurant.color || '#f4f4f4' }"
      >
        <p>{{ restaurant.name }}</p>
      </div>
    </div>

    <!-- Restaurant Details View -->
    <div v-else class="restaurant-details-container">
      <button @click="backToList" class="back-button">Back</button>
      <div class="restaurant-details-view">
        <h2>{{ selectedRestaurant.name }}</h2>
        <p>Rating: {{ selectedRestaurant.rating }}/5</p>
        <p>Location: {{ selectedRestaurant.location }}</p>
        <p>Cuisine: {{ selectedRestaurant.cuisinetype }}</p>
        <p>Opening Hours: {{ selectedRestaurant.openinghours }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "RestaurantList",
  data() {
    return {
      restaurants: [],
      selectedRestaurant: null, // Holds the details of the selected restaurant
    };
  },
  async created() {
    try {
      const response = await axios.get("http://localhost:2905/api/resturants/");
      const allRestaurants = response.data[0].restaurants;

      this.restaurants = allRestaurants.map((restaurant) => ({
        id: restaurant.id,
        name: restaurant.name,
        location: restaurant.location,
        rating: restaurant.rating,
        cuisinetype: restaurant.cuisinetype,
        openinghours: restaurant.openinghours,
      }));
    } catch (error) {
      console.error("Error fetching restaurant details:", error);
    }
  },
  methods: {
    showRestaurantDetails(restaurant) {
      this.selectedRestaurant = restaurant; // Set the selected restaurant
    },
    backToList() {
      this.selectedRestaurant = null; // Reset to show the list
    },
  },
};
</script>

<style scoped>
.restaurant-list-container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  background-color: #fbe1a4;
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border-radius: 10px;
}

.header h1 {
  font-size: 24px;
  margin-bottom: 10px;
}

.restaurant-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.restaurant {
  padding: 15px;
  border-radius: 5px;
  font-size: 18px;
  color: #2c3e50;
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.2s;
}

.restaurant:hover {
  transform: scale(1.05);
}

.restaurant-details-container {
  padding: 20px;
  text-align: left;
}

.back-button {
  display: inline-block;
  margin-bottom: 20px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #3498db;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.back-button:hover {
  background-color: #2980b9;
}

.restaurant-details-view {
  background-color: #f4f4f4;
  padding: 20px;
  border-radius: 5px;
}

.restaurant-details-view h2 {
  margin-bottom: 10px;
}
</style>
