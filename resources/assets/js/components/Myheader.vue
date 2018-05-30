<template>
    <nav class="navbar" role="navigation" aria-label="main navigation">
        <div class="navbar-brand">
            <p>Phonebook</p>
            <div class="navbar-menu is-active">
                <router-link to="/home" class="navbar-item">Home</router-link>
                <router-link to="/about" class="navbar-item">About</router-link>
            </div>
        </div>
    </nav>
</template>

<script>
    export default {
        name: "myheader"
    }
</script>

<style scoped>

</style>