<template>
  <div class="app">
    <h1>🌤 luxsky Weather App</h1>

    <form @submit.prevent="getWeather">
      <input
        v-model="city"
        type="text"
        placeholder="Enter city name,country or region"
        required
      />
      <button type="submit">Get Weather</button>
    </form>

    <div v-if="loading">Loading...</div>

    <div v-if="error" class="error">{{ error }}</div>

    <div v-if="weather">
      <h2>{{ weather.name }}, {{ weather.sys.country }}</h2>
      <p>🌡 Temperature: {{ (weather.main.temp - 273.15).toFixed(1) }} °C</p>
      <p>💧 Humidity: {{ weather.main.humidity }}%</p>
      <p>🌬 Wind Speed: {{ weather.wind.speed }} m/s</p>
      <p>☁ Condition: {{ weather.weather[0].description }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      city: "",
      weather: null,
      error: "",
      loading: false,
    };
  },
  methods: {
    async getWeather() {
      if (!this.city.trim()) {
        this.error = "Please enter a city name.";
        return;
      }

      this.loading = true;
      this.error = "";
      this.weather = null;

      try {
        const apiKey = "e3dc438716e05464c3768720c3ace1e8"
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(
          this.city
        )}&appid=${apiKey}`;

        const response = await axios.get(url);
        this.weather = response.data;
      } catch (err) {
        if (err.response && err.response.status === 404) {
          this.error = "City not found. Please check the spelling.";
        } else {
          this.error = "Failed to fetch weather data. Try again later.";
        }
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>
<style>
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #e0eafc 0%, #cfdef3 100%);
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  color: #2c3e50;
}
.app {
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  padding: 50px 40px;
  border-radius: 40px;
  border: 1px solid rgba(255, 255, 255, 0.8);
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.12);
  width: 100%;
  max-width: 440px;
  text-align: center;
  position: relative;
  transition: transform 0.3s ease;
}
.app::before {
  content: "LUXSKY • WEATHER-CAST";
  position: absolute;
  top: -80px;
  left: 0;
  right: 0;
  font-weight: 800;
  letter-spacing: 5px;
  color: #2c3e50;
  opacity: 0.6;
  font-size: 0.8rem;
}

h1 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 35px;
  color: #34495e;
}
form {
  display: flex;
  background: #ffffff;
  padding: 8px;
  border-radius: 20px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.05);
  margin-bottom: 30px;
}

input {
  flex: 1;
  padding: 15px 20px;
  border: none;
  outline: none;
  font-size: 1.1rem;
  background: transparent;
  color: #2c3e50;
}

input::placeholder {
  color: #bdc3c7;
}
input:hover {
  cursor: text;
}

button {
  background: #2c3e50;
  color: #ffffff;
  border: none;
  padding: 0 25px;
  border-radius: 15px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
button:hover {
  background: #4834d4;
  transform: scale(1.05);
  box-shadow: 0 5px 15px rgba(72, 52, 212, 0.3);
}
h2 {
  font-size: 2rem;
  margin: 10px 0;
  color: #2c3e50;
}

p {
  background: #ffffff;
  margin: 12px 0;
  padding: 18px;
  border-radius: 18px;
  display: flex;
  justify-content: space-between;
  font-weight: 500;
  color: #576574;
  box-shadow: 0 4px 6px rgba(0,0,0,0.02);
  transition: 0.3s ease;
}
p:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.05);
  color: #2c3e50;
}
.error {
  color: #e74c3c;
  background: #fadbd8;
  padding: 12px;
  border-radius: 12px;
  margin-top: 20px;
  font-size: 0.9rem;
}
.app::after {
  content: "TERMINAL SYSTEM © 2026 • ALL RIGHTS RESERVED";
  position: absolute;
  bottom: -60px;
  left: 0;
  right: 0;
  font-size: 0.7rem;
  letter-spacing: 2px;
  color: #7f8c8d;
}
div[v-if="loading"] {
  padding: 20px;
  font-style: italic;
  color: #3498db;
}
</style>


