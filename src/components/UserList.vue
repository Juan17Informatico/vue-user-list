<template>
    <div class="user-list-container">
        <!-- Estado de carga -->
        <div v-if="loading" class="loading-state">
            <v-skeleton-loader v-for="n in 6" :key="n" type="card" class="mb-4" :loading="true">
                <UserCard :user="null" />
            </v-skeleton-loader>
        </div>

        <!-- Estado de error -->
        <v-alert v-else-if="error" type="error" variant="tonal" class="error-alert" closable
            @click:close="error = null">
            <template v-slot:prepend>
                <v-icon icon="mdi-alert-circle" />
            </template>
            <div>
                <strong>Error al cargar usuarios</strong>
                <br />
                <small>{{ error }}</small>
            </div>
            <template v-slot:append>
                <v-btn variant="text" size="small" @click="retryLoad"> Reintentar </v-btn>
            </template>
        </v-alert>

        <!-- Lista de usuarios -->
        <div v-else class="users-grid">
            <!-- Mensaje cuando no hay resultados -->
            <div v-if="filteredUsers.length === 0" class="no-results">
                <v-icon icon="mdi-account-search" size="64" color="grey-lighten-1" />
                <h3 class="text-h6 mt-4 mb-2">No se encontraron usuarios</h3>
                <p class="text-body-2 text-medium-emphasis">
                    Intenta con un término de búsqueda diferente
                </p>
            </div>

            <!-- Grid de tarjetas de usuario -->
            <v-row v-else class="user-cards">
                <v-col v-for="(user, index) in filteredUsers" :key="user.id" cols="12" sm="6" md="4" lg="3">
                    <UserCard :user="user" :index="index" @show-details="openModal" class="user-card-item" />
                </v-col>
            </v-row>
        </div>

        <!-- Modal de detalles -->
        <UserModal :user="selectedUser" v-model="showModal" @close="closeModal" />

        <!-- Botón flotante para scroll hacia arriba -->
        <v-fab v-show="showScrollTop" icon="mdi-chevron-up" location="bottom end" size="small" color="primary"
            @click="scrollToTop" class="scroll-fab" />
    </div>
</template>

<script setup>
import { onMounted, ref, computed, watch, onUnmounted } from "vue";
import { fetchUsers } from "../services/UserService.js";
import UserCard from "./UserCard.vue";
import UserModal from "./UserModal.vue";

const props = defineProps(["search"]);
const emit = defineEmits(["update-count", "loading-change"]);

const users = ref([]);
const loading = ref(true);
const error = ref(null);
const showModal = ref(false);
const selectedUser = ref(null);
const showScrollTop = ref(false);

const filteredUsers = computed(() => {
    const filtered = users.value.filter((u) =>
        u.name.toLowerCase().includes(props.search.toLowerCase())
    );

    // Emitir el conteo actualizado
    emit("update-count", filtered.length);

    return filtered;
});

const openModal = (user) => {
    selectedUser.value = user;
    showModal.value = true;
};

const closeModal = () => {
    showModal.value = false;
    selectedUser.value = null;
};

const retryLoad = async () => {
    loading.value = true;
    error.value = null;
    await loadUsers();
};

const loadUsers = async () => {
    try {
        users.value = await fetchUsers();
        error.value = null;
    } catch (err) {
        error.value = err.message || "Error desconocido al cargar usuarios";
        console.error("Error loading users:", err);
    } finally {
        loading.value = false;
        emit("loading-change", false);
    }
};

const scrollToTop = () => {
    window.scrollTo({
        top: 0,
        behavior: "smooth",
    });
};

const handleScroll = () => {
    showScrollTop.value = window.scrollY > 300;
};

// Lifecycle
onMounted(async () => {
    emit("loading-change", true);
    await loadUsers();

    // Agregar listener para scroll
    window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
    window.removeEventListener("scroll", handleScroll);
});

// Watchers
watch(loading, (newValue) => {
    emit("loading-change", newValue);
});
</script>

<style lang="scss" src="/src/styles/components/UserList.scss" scoped></style>
