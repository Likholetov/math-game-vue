<template>
    <div class="alert alert-secondary">
        <h3>{{ x }} + {{ y }} = ?</h3>
        <hr>
        <div class="buttons">
            <button class="btn btn-success" 
                    v-for="number in answers" 
                    :key="number.id"
                    @click="onAnswer(number)"
            >
                {{ number }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
        props: ['settings'],
        data(){
            return {
                x: mtRand(this.settings.from, this.settings.to),
                y: mtRand(this.settings.from, this.settings.to)
            }
        },
        computed: {
            good(){
                return this.x + this.y
            },
            answers(){
                let res = [this.good]

                while(res.length < this.settings.variants){
                    let num = mtRand (this.good - this.settings.range, this.good + this.settings.range)

                    if(res.indexOf(num) === -1){
                        res.push(num)
                    }
                }

                return res.sort(()=>{return 0.5 - Math.random()})
            } 
        },
        methods: {
            onAnswer(num){
                if(num == this.good){
                    this.$emit('success')
                } else {
                    this.$emit('error', `No! ${this.x} + ${this.y} = ${this.good}!`)
                }
            }
        }
    }

function mtRand(min, max) {
    let diff = max - min
    return Math.floor(Math.random() * (diff + 1)) + min
}
</script>

<style lang="scss" scoped>
    .buttons{
        display: flex;
        justify-content: space-around;
    }

    .btn{
        margin-top: 1rem;
    }

    .alert{
        margin: 3rem;
        padding: 3rem;
    }
</style>