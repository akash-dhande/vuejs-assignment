<template>
    <table>
        <thead>
            <tr>
                <TableHead
                    v-for="head of headers"
                    :key="head.id"
                    :data="head"
                    :on-sort="sort"
                    :sorted-by="sortBy"
                />
            </tr>
        </thead>
        <tbody>
            <TableRow v-for="user of sortedUsers" :key="user.id" :user="user" />
        </tbody>
    </table>
</template>

<script>
import TableRow from './TableRow';
import TableHead from './TableHead';
import {ref} from 'vue';
export default {
    components: {
        TableHead,
        TableRow,
    },
    props: {
        users: {
            type: Array,
            default: () => [],
        }
    },
    setup(props) {
        const sortBy = ref('');
        const sortedUsers = ref([...props.users]);
        const headers = ref([
            {
                heading: 'SlNo',
                type: 'id',
                sortable: true,
            },
            {
                heading: 'Name',
                type: 'name',
                sortable: true,
            },
            {
                heading: 'DOB',
                type: 'dob',
                sortable: true,
            },
            {
                heading: 'Email',
                type: 'email',
                sortable: true,
            },
            {
                heading: 'Location',
                type: 'location',
                sortable: true,
            },
            {
                heading: 'Phone',
                type: 'phone',
                sortable: true,
            },
            {
                heading: 'Picture',
                type: 'picture',
                sortable: false,
            },
        ]);

        function sort(sortInfo) {
            sortBy.value = sortInfo.type;
            sortedUsers.value.sort(function(a, b) {
                let modifier = 1;
                if (sortInfo.asc) modifier = -1;
                if (a[sortInfo.type] > b[sortInfo.type]) return -1 * modifier;
                if (a[sortInfo.type] < b[sortInfo.type]) return 1 * modifier;
                return 0;
            });
        }

        return {
            sort,
            sortBy,
            sortedUsers,
            headers
        };
    }
};
</script>

<style>
table {
    width: 100%;
    border: 1px solid #ccc;
    border-spacing: 0;
}
</style>