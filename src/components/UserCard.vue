<template>
    <v-card>
        <v-card-title>
            <v-avatar size="40" class="me-3">
                <img :src="`https://i.pravatar.cc/150?u=${user.id}`" />
            </v-avatar>
            {{ user.name }}
        </v-card-title>
        <v-card-text>{{ user.email }}</v-card-text>
        <v-card-actions>
            <v-btn @click="$emit('show-details', user)">Ver más</v-btn>
        </v-card-actions>
    </v-card>
</template>

<script setup>
defineProps(["user"]);
defineEmits(["show-details"]);
</script>
