<template>
    <form @submit.prevent="onSubmit">
        <input type="text" placeholder="Новая заметка..." v-model="title">
        <button>Добавить</button>
    </form>
</template>

<script>
export default {
    props: ['todos'],
    data() {
        return {
            title: ''
        }
    },
    methods: {
        onSubmit() {
            if (this.title.length) {
                // const newItemText = this.title.trim()
                const hashGenerator = Math.random().toString(36).slice(2, this.length)
                const newItem = {
                    id: hashGenerator,
                    // index: this.todos.length + 1,
                    title: this.title.trim(),
                    completed: false
                }

                this.title = ''
                this.$emit('add-item', newItem)
            }
        }
    }
}
</script>

<style scoped>
    form {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
    }

    input {
        width: 300px;
        margin-right: 10px;
        border: 1px solid #eee;
        box-shadow: none;
        outline: none;
        padding: 10px 15px;
        font-size: 18px;
        box-sizing: border-box;
    }

    button {
        width: 150px;
        border: 1px solid #eee;
        box-shadow: none;
        outline: none;
        box-sizing: border-box;
        font-size: 18px;
        color: #fff;
        background: #6161ff;
        padding: 10px;
        text-align: center;
    }
</style>