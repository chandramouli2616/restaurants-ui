<template>
  <div class="restaurant-list-container">
    <div class="header">
      <h1>Mouli Restaurant List</h1>
    </div>
    <div v-if="!selectedRestaurant" class="restaurant-list">
      <div
        v-for="restaurant in restaurants"
        :key="restaurant.id"
        class="restaurant"
        :style="{ backgroundColor: restaurant.color || '#f4f4f4' }"
        @click="selectedRestaurant = restaurant"
      >
        <p>{{ restaurant.name }}</p>
      </div>
    </div>
    <div v-else class="restaurant-details-container">
      <button @click="selectedRestaurant = null" class="back-button">Back</button>
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
      selectedRestaurant: null,
    };
  },
  async created() {
    try {
      const { data } = await axios.get("https://resturants-backend-api.onrender.com/api/resturants/");
      this.restaurants = data[0].restaurants.map((restaurant) => ({
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
};
</script>
<style scoped>
.restaurant-list-container {
  font-family: Arial, sans-serif;
  text-align: center;
  background-color: #fbe1a4;
  padding: 20px;
  max-width: 900px;
  margin: 0 auto;
  border-radius: 10px;
}
.header h1 {
  font-size: 24px;
  margin-bottom: 10px;
}
.restaurant-list {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
}
.restaurant {
  padding: 15px;
  border-radius: 5px;
  font-size: 18px;
  color: #2c3e50;
  font-weight: bold;
  cursor: pointer;
  text-align: center;
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
