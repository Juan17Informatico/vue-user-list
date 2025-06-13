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

                <!-- Botones de acción rápida -->
                <v-btn icon variant="text" size="small" @click.stop="callUser" class="action-btn">
                    <v-icon icon="mdi-phone" size="18" />
                    <v-tooltip activator="parent" location="top"> Llamar </v-tooltip>
                </v-btn>

                <v-btn icon variant="text" size="small" @click.stop="emailUser" class="action-btn">
                    <v-icon icon="mdi-email-send" size="18" />
                    <v-tooltip activator="parent" location="top"> Enviar email </v-tooltip>
                </v-btn>
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

    // Aquí podrías agregar lógica para persistir el estado de favorito
    // Por ejemplo, guardar en localStorage o enviar a una API
};

const callUser = () => {
    if (props.user?.phone) {
        window.open(`tel:${props.user.phone}`, "_self");
    }
};

const emailUser = () => {
    if (props.user?.email) {
        window.open(`mailto:${props.user.email}`, "_self");
    }
};
</script>

<style lang="scss" scoped>
.user-card {
    border-radius: 16px;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    cursor: pointer;
    overflow: hidden;
    border: 1px solid rgba(0, 0, 0, 0.05);

    &:hover {
        transform: translateY(-4px);
        box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15) !important;
        border-color: rgba(103, 126, 234, 0.3);
    }

    &:active {
        transform: translateY(-2px);
    }

    &--loading {
        cursor: default;

        &:hover {
            transform: none;
            box-shadow: none !important;
        }
    }
}

.user-card-skeleton {
    padding: 1rem;
}

.user-card-header {
    display: flex;
    align-items: flex-start;
    padding: 1.25rem 1.25rem 0;
    gap: 1rem;

    .avatar-container {
        position: relative;
        flex-shrink: 0;

        .user-avatar {
            transition: transform 0.3s ease;

            .user-card:hover & {
                transform: scale(1.05);
            }
        }

        .status-badge {
            position: absolute;
            bottom: 2px;
            right: 2px;
            width: 14px;
            height: 14px;
            border-radius: 50%;
            border: 2px solid white;

            &.online {
                background-color: #4caf50;
            }
        }
    }

    .user-info {
        flex: 1;
        min-width: 0;

        .user-name {
            font-size: 1.1rem;
            font-weight: 600;
            margin: 0 0 0.25rem 0;
            line-height: 1.3;
            color: rgb(var(--v-theme-on-surface));

            // Truncate long names
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .user-username {
            font-size: 0.875rem;
            color: rgb(var(--v-theme-primary));
            margin: 0;
            font-weight: 500;

            // Truncate long usernames
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
    }

    .favorite-btn {
        flex-shrink: 0;
        transition: transform 0.2s ease;

        &:hover {
            transform: scale(1.1);
        }
    }
}

.user-content {
    padding: 0 1.25rem 0.75rem;

    .info-item {
        display: flex;
        align-items: center;
        gap: 0.75rem;
        margin-bottom: 0.5rem;

        &:last-child {
            margin-bottom: 0;
        }

        .info-icon {
            color: rgb(var(--v-theme-primary));
            opacity: 0.7;
            flex-shrink: 0;
        }

        .info-text {
            font-size: 0.875rem;
            color: rgb(var(--v-theme-on-surface));
            opacity: 0.8;
            line-height: 1.4;

            // Truncate long text
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            flex: 1;
        }
    }
}

.user-actions {
    padding: 0.75rem 1.25rem 1.25rem;
    gap: 0.5rem;

    .details-btn {
        border-radius: 8px;
        text-transform: none;
        font-weight: 500;
        transition: all 0.3s ease;

        &:hover {
            background-color: rgb(var(--v-theme-primary));
            color: white;
        }
    }

    .action-btn {
        transition: all 0.2s ease;

        &:hover {
            background-color: rgba(var(--v-theme-primary-rgb), 0.1);
            transform: scale(1.1);
        }
    }
}

// Dark theme adjustments
.v-theme--dark {
    .user-card {
        border-color: rgba(255, 255, 255, 0.1);

        &:hover {
            border-color: rgba(103, 126, 234, 0.5);
        }
    }

    .status-badge {
        border-color: rgb(var(--v-theme-surface)) !important;
    }
}

// Responsive design
@media (max-width: 768px) {
    .user-card-header {
        padding: 1rem 1rem 0;
        gap: 0.75rem;

        .avatar-container .user-avatar {
            width: 48px !important;
            height: 48px !important;
        }

        .user-info {
            .user-name {
                font-size: 1rem;
            }

            .user-username {
                font-size: 0.8rem;
            }
        }
    }

    .user-content {
        padding: 0 1rem 0.5rem;

        .info-item {
            margin-bottom: 0.4rem;

            .info-text {
                font-size: 0.8rem;
            }
        }
    }
}
</style>
