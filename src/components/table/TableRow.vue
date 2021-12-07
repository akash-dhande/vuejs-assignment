<template>
    <tr>
        <TableColumn v-for="(value, key) of user" :key="key" :data="{value, key}" />
    </tr>
</template>

<script>
import TableColumn from './TableColumn';
export default {
    components: {
        TableColumn,
    },
    props: {
        user: {
            type: Object,
            default: () => {},
        }
    },
};
</script>