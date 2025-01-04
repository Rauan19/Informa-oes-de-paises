<script setup lang="ts">
  import { ref, onMounted } from "vue";
  import api from "../services/api";

  const searchQuery = ref<string>("");  // Variável para armazenar a busca
  const countries = ref<any[]>([]);  // Lista de países
  const filteredCountries = ref<any[]>([]);  // Lista filtrada com base na busca

  // Função para pegar os dados dos países
  const fetchCountries = async () => {
    try {
      const response = await api("/all");
      countries.value = response.data;
      filteredCountries.value = response.data;  // Inicia com todos os países
    } catch (error) {
      console.error("Erro ao buscar países:", error);
    }
  };

  // Função de busca
  const searchCountries = () => {
    if (searchQuery.value.trim() === "") {
      filteredCountries.value = countries.value;  // Se não houver busca, mostra todos os países
    } else {
      filteredCountries.value = countries.value.filter((country) =>
        country.name.common.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
    }
  };

  // Buscar os países assim que o componente for montado
  onMounted(() => {
    fetchCountries();
  });
</script>

<template>
  <div class="container">
    <h1 class="title">Lista de Países</h1>
    
    <div class="search-container">
      <input
        v-model="searchQuery"
        @input="searchCountries"
        type="text"
        placeholder="Buscar por país..."
        class="search-input"
      />
    </div>

    <div v-if="filteredCountries.length === 0" class="no-results">
      <p>Nenhum país encontrado.</p>
    </div>

    <div v-for="(country) in filteredCountries" :key="country.cca3" class="country-card">
      <div class="flag-container">
        <img :src="country.flags.png" :alt="`Bandeira de ${country.name.common}`" class="flag-image" />
      </div>
      
      <div class="country-info">
        <h2 class="country-name">{{ country.name.common }}</h2>
        <p><strong>Capital:</strong> {{ country.capital }}</p>
        <p><strong>Área:</strong> {{ country.area }} km²</p>
        <p><strong>População:</strong> {{ country.population }}</p>
        <a :href="country.maps.googleMaps" target="_blank" class="map-link">Ver no Google Maps</a>
      </div>
    </div>
  </div>
</template>

<style scoped>
  /* Estilos Gerais */
  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    font-family: 'Arial', sans-serif;
    background-color: #f7f7f7;
  }

  .title {
    text-align: center;
    font-size: 2.5rem;
    color: #333;
    margin-bottom: 20px;
  }

  .search-container {
    text-align: center;
    margin-bottom: 30px;
  }

  .search-input {
    width: 100%;
    max-width: 400px;
    padding: 12px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 25px;
    box-sizing: border-box;
    transition: all 0.3s ease;
  }

  .search-input:focus {
    outline: none;
    border-color: #007bff;
  }

  .no-results {
    text-align: center;
    font-size: 1.2rem;
    color: #ff4040;
  }

  /* Cards de Países */
  .country-card {
    display: flex;
    align-items: center;
    background-color: #fff;
    margin-bottom: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    padding: 20px;
    transition: transform 0.3s ease;
  }

  .country-card:hover {
    transform: translateY(-5px);
  }

  .flag-container {
    margin-right: 20px;
  }

  .flag-image {
    width: 60px;
    height: auto;
    border-radius: 5px;
  }

  .country-info {
    flex: 1;
  }

  .country-name {
    font-size: 1.8rem;
    font-weight: bold;
    color: #333;
    margin: 0 0 10px;
  }

  .map-link {
    display: inline-block;
    margin-top: 12px;
    font-size: 14px;
    color: #007bff;
    text-decoration: none;
  }

  .map-link:hover {
    text-decoration: underline;
  }
</style>
