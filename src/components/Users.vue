<template>
    <section>
        <h1>VueJS Assignment</h1>
        <p v-if="loading">Loading Users...</p>
        <p v-if="error">Something went wrong please try again.</p>
        <TableMain v-if="!loading && !error && users" :users="users" />
    </section>
</template>

<script>
import TableMain from './table/TableMain';
import axios from 'axios';
import {ref, onMounted} from "vue";
export default {
    components: {
        TableMain,
    },
    setup() {
        const loading = ref(true);
        const error = ref(false);
        const users = ref([]);

        const transformLocation = (location) => {
            const {street, city, state, country, postcode} = location;
            return `${street.number}, ${street.name}, ${city}, ${state}, ${country}, ${postcode}`;
        };

        const transformUser = (user) => {
            const {name} = user;
            return {
                name: `${name.title} ${name.first} ${name.last}`,
                dob: user.dob.date,
                email: user.email,
                location: transformLocation(user.location),
                phone: user.phone,
                picture: user.picture.thumbnail
            };
        };

        const getUsers = async () => {
            try {
                const response = await axios.get('https://randomuser.me/api/?results=50');
                loading.value = false;
                const rawUsers = [];
                response.data.results.map((u, index)=>rawUsers.push({id: index + 1, ...transformUser(u)}));
                users.value = rawUsers;
            } catch {
                loading.value = false;
                error.value = true;
            }
        };

        onMounted(() => {
            getUsers();
        });

        return {
            error,
            loading,
            users
        };

    }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
</style>
