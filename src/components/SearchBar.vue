<template>
    <div class="search-container">
        <v-text-field
            :model-value="modelValue"
            @update:modelValue="handleInput"
            label="Buscar por nombre de usuario..."
            placeholder="Ej: John Doe"
            prepend-inner-icon="mdi-magnify"
            clearable
            variant="outlined"
            color="primary"
            class="search-field"
            :loading="isSearching"
            hide-details="auto"
            density="comfortable"
        >
            <template v-slot:append-inner>
                <v-fade-transition>
                    <v-icon
                        v-if="modelValue && !isSearching"
                        icon="mdi-check-circle"
                        color="success"
                        size="small"
                    />
                </v-fade-transition>
            </template>
        </v-text-field>

        <!-- Sugerencias de búsqueda (opcional) -->
        <div v-if="showSuggestions" class="search-suggestions">
            <v-chip-group>
                <v-chip
                    v-for="suggestion in suggestions"
                    :key="suggestion"
                    size="small"
                    variant="outlined"
                    @click="selectSuggestion(suggestion)"
                    class="suggestion-chip"
                >
                    <v-icon start icon="mdi-trending-up" size="small" />
                    {{ suggestion }}
                </v-chip>
            </v-chip-group>
        </div>
    </div>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps(["modelValue"]);
const emit = defineEmits(["update:modelValue"]);

const isSearching = ref(false);
const showSuggestions = ref(false);
const suggestions = ref(["John", "Jane", "Admin", "User", "Test"]);

let searchTimeout = null;

const handleInput = (value) => {
    emit("update:modelValue", value);

    // Simular búsqueda con debounce
    isSearching.value = true;

    if (searchTimeout) {
        clearTimeout(searchTimeout);
    }

    searchTimeout = setTimeout(() => {
        isSearching.value = false;
    }, 300);

    // Mostrar sugerencias cuando el campo está vacío
    showSuggestions.value = !value || value.length === 0;
};

const selectSuggestion = (suggestion) => {
    emit("update:modelValue", suggestion);
    showSuggestions.value = false;
};

// Ocultar sugerencias cuando se escribe
watch(
    () => props.modelValue,
    (newValue) => {
        if (newValue && newValue.length > 0) {
            showSuggestions.value = false;
        }
    }
);
</script>

<style lang="scss" scoped>
.search-container {
    position: relative;
    max-width: 600px;
    margin: 0 auto;
}

.search-field {
    :deep(.v-field) {
        border-radius: 12px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);

        &:hover {
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            transform: translateY(-1px);
        }

        &.v-field--focused {
            box-shadow: 0 4px 20px rgba(103, 126, 234, 0.25);
            transform: translateY(-2px);
        }
    }

    :deep(.v-field__input) {
        font-size: 1rem;
        padding: 12px 16px;
    }

    :deep(.v-field__prepend-inner) {
        padding-top: 0;

        .v-icon {
            opacity: 0.6;
            transition: opacity 0.3s ease;
        }
    }

    &:hover :deep(.v-field__prepend-inner .v-icon) {
        opacity: 1;
    }
}

.search-suggestions {
    margin-top: 1rem;
    text-align: center;

    .suggestion-chip {
        margin: 0.25rem;
        transition: all 0.3s ease;
        cursor: pointer;

        &:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
        }
    }
}

// Animaciones de entrada
.search-container {
    animation: slideInDown 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

@keyframes slideInDown {
    from {
        opacity: 0;
        transform: translateY(-20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

// Responsive
@media (max-width: 768px) {
    .search-container {
        max-width: 100%;
        padding: 0 1rem;
    }

    .search-field :deep(.v-field__input) {
        font-size: 0.9rem;
        padding: 10px 14px;
    }

    .search-suggestions {
        .suggestion-chip {
            font-size: 0.8rem;
        }
    }
}
</style>
