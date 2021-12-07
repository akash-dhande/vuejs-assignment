<template>
    <th
        :class="{
            'sortable': data.sortable,
            'sorted': sortedBy === data.type,
            'desc': orderDesc,
            'asc': !orderDesc
        }"
        @click="sort(data)">
        {{data.heading}}
    </th>
</template>
<script>
import {ref} from 'vue';
export default {
    props: {
        data: {
            type: Object,
            default: () => {},
        },
        sortedBy: {
            type: String,
            default: '',
        },
        onSort: {
            type: Function,
            default: () => {}
        }
    },
    setup(props) {
        const orderDesc = ref(false);

        function sort(sortInfo) {
            props.onSort({type: sortInfo.type, asc: orderDesc.value});
            orderDesc.value = !orderDesc.value;
        }

        return {
            orderDesc,
            sort
        };
    }
};
</script>

<style>
th {
    background: #ccc;
    padding: 0.5rem;
}
th.sortable {
    cursor: pointer;
}
th.sortable:not(.sorted):after {
    content: "▼";
    opacity: 0.4;
}
th.sorted.asc:after {
    content: "▲";
}
th.sorted.desc:after {
    content: "▼";
}
</style>