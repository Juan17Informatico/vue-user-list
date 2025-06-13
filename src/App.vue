<template>
    <v-app>
        <v-container>
            <SearchBar v-model="searchQuery" />
            <UserList :search="searchQuery" />
        </v-container>
    </v-app>
</template>

<script setup>
import { ref } from "vue";
import SearchBar from "./components/SearchBar.vue";
import UserList from "./components/UserList.vue";

const searchQuery = ref("");
</script>
