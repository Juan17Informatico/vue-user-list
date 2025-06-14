<template>
    <v-card
        class="user-card"
        :class="{ 'user-card--loading': !user }"
        elevation="2"
        @click="!user ? null : $emit('show-details', user)"
        :ripple="!!user"
    >
        <!-- Skeleton loading state -->
        <template v-if="!user">
            <v-skeleton-loader type="avatar, sentences, actions" class="user-card-skeleton" />
        </template>

        <!-- Contenido real -->
        <template v-else>
            <!-- Header con avatar y nombre -->
            <div class="user-card-header">
                <div class="avatar-container">
                    <v-avatar size="56" class="user-avatar">
                        <v-img
                            :src="`https://i.pravatar.cc/150?u=${user.id}`"
                            :alt="`Avatar de ${user.name}`"
                            cover
                        >
                            <template v-slot:placeholder>
                                <v-skeleton-loader type="avatar" />
                            </template>
                        </v-img>
                    </v-avatar>

                    <!-- Badge de estado online (decorativo) -->
                    <div class="status-badge online"></div>
                </div>

                <div class="user-info">
                    <h3 class="user-name">{{ user.name }}</h3>
                    <p class="user-username">@{{ user.username }}</p>
                </div>

                <!-- Botón de favorito -->
                <v-btn
                    icon
                    variant="text"
                    size="small"
                    class="favorite-btn"
                    @click.stop="toggleFavorite"
                >
                    <v-icon
                        :icon="isFavorite ? 'mdi-heart' : 'mdi-heart-outline'"
                        :color="isFavorite ? 'red' : 'grey'"
                        size="20"
                    />
                </v-btn>
            </div>

            <!-- Contenido principal -->
            <v-card-text class="user-content">
                <!-- Email con icono -->
                <div class="info-item">
                    <v-icon icon="mdi-email" size="16" class="info-icon" />
                    <span class="info-text">{{ user.email }}</span>
                </div>

                <!-- Ubicación -->
                <div class="info-item">
                    <v-icon icon="mdi-map-marker" size="16" class="info-icon" />
                    <span class="info-text">{{ user.address.city }}</span>
                </div>

                <!-- Compañía -->
                <div class="info-item">
                    <v-icon icon="mdi-domain" size="16" class="info-icon" />
                    <span class="info-text">{{ user.company.name }}</span>
                </div>
            </v-card-text>

            <!-- Footer con acciones -->
            <v-card-actions class="user-actions">
                <v-btn
                    variant="outlined"
                    size="small"
                    prepend-icon="mdi-information"
                    @click.stop="$emit('show-details', user)"
                    class="details-btn"
                >
                    Ver detalles
                </v-btn>

                <v-spacer />

            </v-card-actions>
        </template>
    </v-card>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps(["user", "index"]);
const emit = defineEmits(["show-details"]);

const isFavorite = ref(false);

const toggleFavorite = () => {
    isFavorite.value = !isFavorite.value;
};

</script>

<style lang="scss" src="/src/styles/components/UserCard.scss" scoped></style>
