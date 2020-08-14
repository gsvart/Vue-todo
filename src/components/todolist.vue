<template>
    <div>
        <ul>
            <todoitem 
                v-for="(item, index) in todos" 
                v-bind:item="item"
                v-bind:index="index"
                v-on:remove-item="removeTodo" />
        </ul>
    </div>
</template>

<script>
import todoitem from '@/components/todoitem'

export default {
    props: ['todos'],
    components: {
        todoitem
    },
    methods: {
        removeTodo(id) {
            this.$emit('remove-item', id)
        }
    }
}
</script>

<style scoped>
    ul {
        list-style: none;
        padding: 0;
        margin: 0 auto;
        display: flex;
        flex-direction: column;
    }
</style>