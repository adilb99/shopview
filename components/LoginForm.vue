<template>
    <div class="login_form">
        <slot></slot>
    </div>
</template>
<style scoped>
    .login_form {
        box-shadow: 2px 5px 5px #b9b9b9;
        background: white;
        border: 1px solid #e5e5e5;
        width: 30%;
        margin-left: auto;
        margin-right: auto;
        text-align: center;
        padding-bottom: 2.5em;
    }
</style>