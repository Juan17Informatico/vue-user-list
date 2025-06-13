<template>
    <v-row>
        <v-col v-for="user in filteredUsers" :key="user.id" cols="12" sm="6" md="4">
            <UserCard :user="user" @show-details="openModal" />
        </v-col>

        <UserModal :user="selectedUser" v-model="showModal" />
        <v-progress-circular v-if="loading" indeterminate color="primary" />
        <v-alert v-if="error" type="error">{{ error }}</v-alert>
    </v-row>
</template>

<script setup>
import { onMounted, ref, computed } from "vue";
import { fetchUsers } from "../services/UserService.js";
import UserCard from "./UserCard.vue";
import UserModal from "./UserModal.vue";

const props = defineProps(["search"]);
const users = ref([]);
const loading = ref(true);
const error = ref(null);
const showModal = ref(false);
const selectedUser = ref(null);

const filteredUsers = computed(() =>
    users.value.filter((u) => u.name.toLowerCase().includes(props.search.toLowerCase()))
);

const openModal = (user) => {
    selectedUser.value = user;
    showModal.value = true;
};

onMounted(async () => {
    try {
        users.value = await fetchUsers();
    } catch (err) {
        error.value = err.message;
    } finally {
        loading.value = false;
    }
});
</script>
