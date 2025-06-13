<script setup>
const props = defineProps(["modelValue"]);
const emit = defineEmits(["update:modelValue"]);
</script>

<template>
    <v-text-field
        :model-value="modelValue"
        @update:modelValue="emit('update:modelValue', $event)"
        label="Buscar usuario"
        prepend-icon="mdi-magnify"
        clearable
    />
</template>
