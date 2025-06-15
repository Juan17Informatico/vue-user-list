<template>
    <div class="search-bar-container">
        <!-- Main Search Input -->
        <div class="search-input-wrapper">
            <div class="search-input-container">
                <v-text-field :model-value="modelValue" @update:modelValue="emit('update:modelValue', $event)"
                    label="Buscar por nombre..." prepend-inner-icon="mdi-magnify" clearable variant="outlined"
                    color="primary" density="comfortable" hide-details class="search-input"
                    :class="{ 'input-focused': isFocused }" @focus="isFocused = true" @blur="isFocused = false" />
                <div class="search-input-glow"></div>
            </div>

            <!-- Search Icon Animation -->
            <div class="search-icon-decoration">
                <v-icon icon="mdi-magnify" size="20" class="floating-search-icon"
                    :class="{ 'icon-active': modelValue }" />
            </div>
        </div>

        <!-- Quick Search Suggestions -->
        <v-expand-transition>
            <div v-if="showSuggestions" class="suggestions-section">
                <div class="suggestions-header">
                    <v-icon icon="mdi-lightning-bolt" size="16" class="suggestions-icon" />
                    <span class="suggestions-title">Búsquedas populares</span>
                </div>

                <div class="suggestions-container">
                    <v-chip v-for="suggestion in suggestions" :key="suggestion" class="suggestion-chip" size="small"
                        variant="tonal" color="primary" :prepend-icon="suggestion.icon"
                        @click="selectSuggestion(suggestion)" @mouseenter="hoveredSuggestion = suggestion"
                        @mouseleave="hoveredSuggestion = null"
                        :class="{ 'chip-hovered': hoveredSuggestion === suggestion }">
                        {{ suggestion.name }}
                    </v-chip>
                </div>

                <!-- Suggestion Helper Text -->
                <div class="suggestions-helper">
                    <v-icon icon="mdi-information-outline" size="14" class="helper-icon" />
                    <span class="helper-text">Haz clic en cualquier nombre para buscar</span>
                </div>
            </div>
        </v-expand-transition>

        <!-- Search Stats -->
        <v-fade-transition>
            <div v-if="modelValue && modelValue.length > 0" class="search-stats">
                <div class="stats-container">
                    <v-icon icon="mdi-chart-line" size="16" class="stats-icon" />
                    <span class="stats-text">Buscando: "{{ modelValue }}"</span>
                    <v-btn icon="mdi-close" size="x-small" variant="text" class="clear-btn" @click="clearSearch" />
                </div>
            </div>
        </v-fade-transition>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps(['modelValue']);
const emit = defineEmits(['update:modelValue']);

const isFocused = ref(false);
const hoveredSuggestion = ref(null);

const suggestions = [
    { name: 'Chelsey', icon: 'mdi-account-circle' },
    { name: 'Kurtis', icon: 'mdi-account-star' },
    { name: 'Glen', icon: 'mdi-account-heart' }
];

const showSuggestions = computed(() => !props.modelValue || props.modelValue.length === 0);

const selectSuggestion = (suggestion) => {
    emit('update:modelValue', suggestion.name);
};

const clearSearch = () => {
    emit('update:modelValue', '');
};
</script>

<style src="/src/styles/components/SearchBar.scss" scoped></style>