<template>
    <li class="list_item">
        <span v-bind:class="{done: item.completed}">
            <input type="checkbox" 
                    @change.prevent="item.completed = !item.completed"
                    :checked="item.completed">
            <strong>{{index + 1}}</strong>
            <div class="item_title">{{item.title | uppercase}}</div>
        </span>
        <button class="close_btn" 
                @click="$emit('remove-item', item.id)">&times</button>
    </li>
</template>

<script>
export default {
    // props: ['item'] Обычная запись
    
    // Запись с проверкой
    props: {
        item: {
            type: Object,
            required: true
        },
        index: Number
    },
    filters: {
        uppercase(value) {
            return value.toUpperCase()
        }
    }
}
</script>

<style scoped>
    .list_item {
        text-align: left;
        margin-bottom: 15px;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        border: 1px solid #eee;
        padding: 10px;
    }

    .list_item:last-child {
        margin-bottom: 0;
    }

    .list_item span {
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        align-items: flex-end;
    }

    .close_btn {
        background: #ff6363;
        color: #fff;
        border: none;
        outline: none;
        line-height: 100%;
        font-size: 18px;
        cursor: pointer;
        font-weight: bold;
    }

    .done .item_title {
        text-decoration: line-through;
    }

    input {
        margin-right: 10px;
        width: 20px;
        height: 20px;
    }

    strong {
        margin-right: 10px;
    }
</style>