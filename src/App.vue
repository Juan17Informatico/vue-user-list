<template>
    <v-app>
        <!-- Header con gradiente -->
        <v-app-bar
            :elevation="0"
            class="app-header"
            height="80"
            scroll-behavior="elevate"
        >
            <v-container class="d-flex align-center">
                <div class="d-flex align-center">
                    <v-icon 
                        icon="mdi-account-group" 
                        size="32" 
                        color="white"
                        class="me-3"
                    />
                    <div>
                        <h1 class="app-title">UserHub</h1>
                        <p class="app-subtitle">Directorio de usuarios</p>
                    </div>
                </div>
            </v-container>
        </v-app-bar>

        <!-- Contenido principal -->
        <v-main>
            <v-container class="main-container">
                <!-- Sección de búsqueda con estilo moderno -->
                <div class="search-section">
                    <SearchBar v-model="searchQuery" />
                    <div class="search-stats" v-if="!loading">
                        <v-chip 
                            color="primary" 
                            variant="tonal" 
                            size="small"
                            prepend-icon="mdi-account-multiple"
                        >
                            {{ filteredCount }} usuarios encontrados
                        </v-chip>
                    </div>
                </div>

                <!-- Lista de usuarios -->
                <div class="user-content">
                    <UserList 
                        :search="searchQuery" 
                        @update-count="updateFilteredCount"
                        @loading-change="loading = $event"
                    />
                </div>
            </v-container>
        </v-main>

        <!-- Footer -->
        <v-footer class="footer">
            <v-container>
                <div class="text-center">
                    <small class="text-medium-emphasis">
                        © 2025 UserHub - Desarrollado con Vue 3 & Vuetify
                    </small>
                </div>
            </v-container>
        </v-footer>
    </v-app>
</template>

<script setup>
import { ref } from "vue";
import SearchBar from "./components/SearchBar.vue";
import UserList from "./components/UserList.vue";

const searchQuery = ref("");
const filteredCount = ref(0);
const loading = ref(true);

const updateFilteredCount = (count) => {
    filteredCount.value = count;
};
</script>

<style lang="scss" scoped>
.app-header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    
    .app-title {
        font-size: 1.5rem;
        font-weight: 700;
        color: white;
        margin: 0;
        line-height: 1.2;
    }
    
    .app-subtitle {
        font-size: 0.875rem;
        color: rgba(255, 255, 255, 0.8);
        margin: 0;
        line-height: 1;
    }
}

.main-container {
    padding-top: 2rem;
    padding-bottom: 2rem;
    min-height: calc(100vh - 160px);
}

.search-section {
    margin-bottom: 2rem;
    
    .search-stats {
        margin-top: 1rem;
        display: flex;
        justify-content: center;
    }
}

.user-content {
    position: relative;
}

.footer {
    background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
    margin-top: auto;
}

// Animaciones suaves
.v-main {
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

// Responsive design
@media (max-width: 768px) {
    .main-container {
        padding-top: 1rem;
        padding-bottom: 1rem;
    }
    
    .app-title {
        font-size: 1.25rem !important;
    }
    
    .app-subtitle {
        font-size: 0.8rem !important;
    }
}
</style>