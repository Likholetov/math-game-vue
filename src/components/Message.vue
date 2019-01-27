<template>
   <div class="alert" :class="'alert-' + type">
        <h3>{{ text }}</h3>
        <hr>
        <div class="buttons">
            <button class="btn"
               :class="'btn-' + type" 
               @click="$emit('onNext')"
            >Continue</button>
        </div>
    </div>
</template>

<script>
export default {
   props: ['type', 'text']
}
</script>

<style lang="scss" scoped>
    .buttons{
        display: flex;
        justify-content: space-around;
    }

    .btn{
        margin-top: 1rem
    }

    .alert{
        margin: 3rem;
        padding: 3rem;
    }
</style>