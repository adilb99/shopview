<template>
    <div>
        <h1> MY ORDERS HERE </h1>

        <button v-on:click="counter += 1"> Add 1 </button>

        <p> Counter: {{ counter }} </p>

    </div>
</template>
<script>
export default {
    data() {
        return {
            counter: 0
        }
    }
}
</script>