<template>
    <v-dialog :model-value="modelValue" @update:modelValue="$emit('update:modelValue', $event)" max-width="600"
        :transition="isMobile ? 'dialog-bottom-transition' : 'dialog-transition'" :fullscreen="isMobile" persistent
        class="user-modal">
        <v-card v-if="user" class="user-modal-card">
            <!-- Header con fondo gradiente -->
            <div class="modal-header">
                <div class="header-background"></div>
                <div class="header-content">
                    <!-- Avatar grande -->
                    <div class="avatar-section">
                        <v-avatar size="100" class="user-avatar-large">
                            <v-img :src="`https://i.pravatar.cc/200?u=${user.id}`" :alt="`Avatar de ${user.name}`"
                                cover>
                                <template #placeholder>
                                    <v-skeleton-loader type="avatar" />
                                </template>
                            </v-img>
                        </v-avatar>
                    </div>

                    <!-- Información principal -->
                    <div class="user-main-info">
                        <h2 class="user-name">{{ user.name }}</h2>
                        <p class="user-username">@{{ user.username }}</p>
                    </div>

                    <!-- Botón cerrar -->
                    <v-btn icon variant="text" size="small" @click="closeModal" class="close-btn">
                        <v-icon icon="mdi-close" color="white" />
                    </v-btn>
                </div>
            </div>

            <!-- Contenido principal -->
            <v-card-text class="modal-content">
                <!-- Información de contacto -->
                <div class="info-section">
                    <h3 class="section-title">
                        <v-icon icon="mdi-account-circle" class="section-icon" />
                        Información de Contacto
                    </h3>

                    <div class="info-grid">
                        <!-- Email -->
                        <div class="info-item">
                            <div class="info-label">
                                <v-icon icon="mdi-email" size="20" color="primary" />
                                <span>Email</span>
                            </div>
                            <div class="info-value">
                                <a :href="`mailto:${user.email}`" class="contact-link">
                                    {{ user.email }}
                                </a>
                            </div>
                        </div>

                        <!-- Teléfono -->
                        <div class="info-item">
                            <div class="info-label">
                                <v-icon icon="mdi-phone" size="20" color="primary" />
                                <span>Teléfono</span>
                            </div>
                            <div class="info-value">
                                <a :href="`tel:${user.phone}`" class="contact-link">
                                    {{ user.phone }}
                                </a>
                            </div>
                        </div>

                        <!-- Sitio web -->
                        <div class="info-item">
                            <div class="info-label">
                                <v-icon icon="mdi-web" size="20" color="primary" />
                                <span>Sitio Web</span>
                            </div>
                            <div class="info-value">
                                <a :href="`https://${user.website}`" target="_blank" rel="noopener noreferrer"
                                    class="contact-link">
                                    {{ user.website }}
                                    <v-icon icon="mdi-open-in-new" size="14" class="ml-1" />
                                </a>
                            </div>
                        </div>
                    </div>
                </div>

                <v-divider class="my-6" />

                <!-- Información de ubicación -->
                <div class="info-section">
                    <h3 class="section-title">
                        <v-icon icon="mdi-map-marker" class="section-icon" />
                        Ubicación
                    </h3>

                    <div class="location-info">
                        <div class="address-card">
                            <div class="address-details">
                                <p class="address-line">{{ user.address.street }}</p>
                                <p class="address-line">{{ user.address.suite }}</p>
                                <p class="address-line">
                                    {{ user.address.city }}, {{ user.address.zipcode }}
                                </p>
                            </div>
                            <v-btn variant="outlined" size="small" prepend-icon="mdi-map" @click="openMap"
                                class="map-btn">
                                Ver en mapa
                            </v-btn>
                        </div>
                    </div>
                </div>

                <v-divider class="my-6" />

                <!-- Información profesional -->
                <div class="info-section">
                    <h3 class="section-title">
                        <v-icon icon="mdi-domain" class="section-icon" />
                        Información Profesional
                    </h3>

                    <div class="company-info">
                        <div class="company-card">
                            <div class="company-details">
                                <h4 class="company-name">{{ user.company.name }}</h4>
                                <p class="company-catchphrase">{{ user.company.catchPhrase }}</p>
                                <p class="company-bs">{{ user.company.bs }}</p>
                            </div>
                        </div>
                    </div>
                </div>
            </v-card-text>

            <!-- Solo botón para cerrar -->
            <v-card-actions class="modal-actions justify-end">
                <v-btn variant="text" @click="closeModal" class="close-text-btn">Cerrar</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script setup>
import { computed } from "vue";
import { useDisplay } from "vuetify";

const props = defineProps(["user", "modelValue"]);
const emit = defineEmits(["update:modelValue", "close"]);

const { mobile } = useDisplay();
const isMobile = computed(() => mobile.value);

const closeModal = () => {
    emit("update:modelValue", false);
    emit("close");
};

const openMap = () => {
    if (props.user?.address) {
        const address = `${props.user.address.street}, ${props.user.address.city}`;
        const encodedAddress = encodeURIComponent(address);
        window.open(`https://www.google.com/maps/search/?api=1&query=${encodedAddress}`, "_blank");
    }
};
</script>

<style lang="scss" src="./src/styles/components/UserModal.scss"></style>
