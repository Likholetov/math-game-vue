<template>
    <div class="alert alert-primary">
        <h3>Correct {{ stats.success }}</h3>
        <h3>Incorrect {{ stats.error }}</h3>
        <hr>
        <button class="btn btn-secondary" @click="$emit('repeat')">Repeat</button>
        <button class="btn btn-primary" @click="$emit('nextLevel')">Next</button>
    </div>
</template>

<script>
export default {
    props: ['stats']
}
</script>

<style lang="scss" scoped>
    .btn{
        margin-top: 1rem
    }

    .alert{
        margin: 3rem;
        padding: 3rem;
    }
</style>