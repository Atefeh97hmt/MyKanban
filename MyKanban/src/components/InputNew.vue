<script setup>
    import { ref, defineEmits } from 'vue';
    const text = ref("");
    const emits = defineEmits(["onNewItem"]);
    function handleSubmit() {
        if (text != "") {
            emits("onNewItem", text);
            text.value = "";
        }
    }
</script>

<template>
    <form action="" @submit.prevent="handleSubmit">
        <input class="inputNew" type="text" name="inputNew" id="inputNew" v-model="text" placeholder="Add a New Task + Enter">
    </form>
</template>

<style scoped>
    .inputNew {
    width: 91%;
    border: none;
    padding: 0.5rem;
    text-align: center;
    outline: none;
    font-weight: lighter;
    margin: 6px;
    }
</style>