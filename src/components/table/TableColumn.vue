<template>
    <td>
        <img v-if="data.key === 'picture'" :src="data.value" />
        <span v-else-if="data.key === 'dob'">{{formatDate(data.value)}}</span>
        <span v-else>{{data.value}}</span>
    </td>
</template>

<script>
import moment from 'moment';
export default {
    props: {
        data: {
            type: Object,
            default: () => {},
        }
    },
    setup() {
        function formatDate(timestamp) {
            return moment(timestamp).format('L');
        }

        return {
            formatDate,
        };
    }
};
</script>

<style scoped>
td {
    padding: 0.5rem;
    border-bottom: 1px solid #ccc;
}
</style>