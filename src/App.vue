<template>
    <v-app>
        <!-- Header -->
        <v-app-bar flat class="app-header" height="100" elevation="0" style="background: transparent;">
            <div class="header-background"></div>
            <v-container class="d-flex align-center position-relative">
                <div class="header-icon-container">
                    <v-icon icon="mdi-account-group" size="40" class="header-icon" />
                </div>
                <div class="header-content">
                    <h1 class="header-title">Juan Campuzano</h1>
                    <p class="header-subtitle">Explora y conecta con usuarios</p>
                </div>
                <v-spacer />
                <!-- Decorative elements -->
                <div class="header-decoration">
                    <div class="decoration-circle circle-1"></div>
                    <div class="decoration-circle circle-2"></div>
                    <div class="decoration-circle circle-3"></div>
                </div>
            </v-container>
        </v-app-bar>

        <!-- Main Content -->
        <v-main class="main-content">
            <div class="content-background"></div>
            <v-container class="py-8 position-relative">
                <!-- Search Section -->
                <div class="search-section">
                    <div class="search-container">
                        <h2 class="search-title">Encuentra usuarios</h2>
                        <p class="search-description">Busca y explora perfiles de usuarios</p>
                        <SearchBar v-model="searchQuery" class="search-bar" />

                        <!-- Results Counter -->
                        <v-fade-transition>
                            <div v-if="!loading" class="results-counter">
                                <v-chip color="primary" variant="elevated" size="large"
                                    prepend-icon="mdi-account-multiple" class="counter-chip">
                                    <span class="counter-text">{{ filteredCount }} usuarios encontrados</span>
                                </v-chip>
                            </div>
                        </v-fade-transition>
                    </div>
                </div>

                <!-- User List -->
                <div class="user-list-section">
                    <UserList :search="searchQuery" @update-count="filteredCount = $event"
                        @loading-change="loading = $event" />
                </div>
            </v-container>
        </v-main>

        <!-- Footer -->
        <v-footer class="app-footer">
            <v-container class="footer-content">
                <div class="footer-brand">
                    <v-icon icon="mdi-account-group" size="24" class="me-2" />
                    <span class="footer-title">JuanCM</span>
                </div>
                <div class="footer-info">
                    <span class="footer-text">© 2025 Juan Campuzano</span>
                    <v-divider vertical class="mx-3" />
                    <span class="footer-tech">Vue 3 & Vuetify</span>
                </div>
            </v-container>
        </v-footer>
    </v-app>
</template>

<script setup>
import { ref } from 'vue';
import SearchBar from './components/SearchBar.vue';
import UserList from './components/UserList.vue';

const searchQuery = ref('');
const filteredCount = ref(0);
const loading = ref(true);
</script>

<style scoped src="/src/styles/components/App.scss"></style>