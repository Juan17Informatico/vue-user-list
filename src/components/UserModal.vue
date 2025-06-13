<template>
    <v-dialog
        :model-value="modelValue"
        @update:modelValue="$emit('update:modelValue', $event)"
        max-width="500"
        transition="dialog-bottom-transition"
    >
        <v-card v-if="user">
            <v-card-title>{{ user.name }}</v-card-title>
            <v-card-text>
                <p>
                    <strong>Dirección:</strong> {{ user.address.street }}, {{ user.address.city }}
                </p>
                <p><strong>Teléfono:</strong> {{ user.phone }}</p>
                <p><strong>Compañía:</strong> {{ user.company.name }}</p>
                <p>
                    <strong>Sitio web:</strong>
                    <a :href="'http://' + user.website" target="_blank">{{ user.website }}</a>
                </p>
            </v-card-text>
            <v-card-actions>
                <v-btn @click="$emit('update:modelValue', false)">Cerrar</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script setup>
defineProps(["user", "modelValue"]);
defineEmits(["update:modelValue"]);
</script>
