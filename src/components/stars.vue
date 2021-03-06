<template>
    <div>
        <i class="fas fa-star"
           v-for="(n, index) in 5"
           v-bind:key="index"
           v-bind:style="changeColor(star, index)"
           v-on:click="onStar(index)"
        ></i>
    </div>
</template>

<script>
export default {
    name: 'Star',
    props: ['star'],
    data() {
        return {
            startCount: this.star
        }
    },
    filters: {
    },
    methods: {
        changeColor (star, index) {
            let style = {};
            // if(index === 3) {
            //     style.backgroundColor = 'green'
            // }

            if(star > index) {
                style.color = 'red'
            }
            return style;
        },
        onStar(index) {
            this.$emit('click', {index: index})
        }
    }
}
</script>

<style scoped>
    .red{
        color: red;
    }
    .green {
        color: green;
    }
</style>