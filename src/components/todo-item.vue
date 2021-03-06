<template>
    <tr>
        <td>
            <input type="checkbox" v-on:change="onChange" v-bind:checked="prop.done"
            >
        </td>
        <td>
            <span v-bind:style="prop.done === true ? checkedStyle : checkedStyleNone"
                  v-bind:title="prop.descript"
                  v-on:click="onModal"
            >
                {{   prop.name   }}
            </span>
        </td>
        <td>
            <Star v-bind:star="prop.star"
                  v-on:click="starAdd($event)"
            ></Star>
        </td>
        <td>
            <i class="fas fa-trash-alt" v-on:click="onDelete"
            ></i>
        </td>

    </tr>
</template>

<script>
import {bus} from '../main'
import Star from '../components/stars'

export default {
    name: 'TodoItem',
    components: {
        Star
    },
    props: ["prop"],
    data() {
        return {
            checked: false,
            checkedStyle: {
                backgroundColor: '#bdbdbd',
                color: '#546e7a',
                textDecoration: 'line-through'
            },
            checkedStyleNone: {
                backgroundColor: '#eceff1',
                color: '#37474f',
                textDecoration: 'none'
            },
            filtred: [],

        }
    },
    created() {

    },
    methods: {

        onChange: function () {
            this.$emit('change', {
                check: !this.prop.done
            })
        },
        onDelete: function () {
            this.$emit('delete');
        },
        onModal: function () {
            this.$emit('modal');
            bus.$emit('editDisabled',
                {
                    data: this.prop,
                    btn: 'Edit'
                })
        },
        starAdd($event) {
            this.$emit('star', $event)
        }
    }
}
</script>

<style scoped>

</style>