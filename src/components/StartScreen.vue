<template>
    <div class="alert alert-secondary">
        <h3>Are you ready?</h3>
        <button class="btn btn-success" @click="$emit('onStart')">
            Start
        </button>
    </div>
</template>

<script>
export default {
    
}
</script>

<style lang="scss" scoped>
    .btn{
        margin-top: 1rem;
    }

    .alert{
        margin: 3rem;
        padding: 3rem;
    }
</style>
