<template>
    <li class="list-group-item d-flex justify-content-between align-items-center">
        <input type="checkbox" v-on:change="onChange" v-bind:checked="prop.done"
        >
        <span v-bind:style="prop.done === true ? checkedStyle : checkedStyleNone"
              v-bind:title="prop.descript"
              v-on:click="onModal"
        >
                    {{ prop.name }}
    </span>
        <div class="starRole" v-on:click="onStar">


        </div>
        <i class="fas fa-trash-alt" v-on:click="onDelete"
        ></i>
    </li>
</template>

<script>
import {bus} from '../main'

export default {
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
    mounted() {
        // console.log(this.prop.star, '  - prop star');
        // let arrStar = [...document.querySelectorAll('.starRole i')];
        // console.log(arrStar.length, ' - arrStar');
        // if (this.prop.star !== '') {
        // for (let i = 0; i <= arrStar.length; i++) {
        //         console.log('!!!');
        //         // if (i < this.prop.star) {
        //         //     this.arrStar[i].classList.add('starRoleActive')
        //         // }
        //     }
        // }
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
            console.log('SPAN click');
            this.$emit('modal');
            bus.$emit('editDisabled',
                {
                    data: this.prop,
                    btn: 'Edit'
                })

        },
        onStar($event) {
            console.log($event.target, ' - ');
            // $event.target.style.color = 'red';
            // let index = arr.indexOf($event.target);
            // if(this.prop.star !== '') {
            // 	for(let i = 0; i <= arr.length; ++i){
            // 		if(i <= index) {
            // 			console.log(arr[i].innerHTML, '  -arr[i] < index');
            // 			arr[i].style.backgroundColor = 'red'
            // 		} else {
            // 			console.log(arr[i].innerHTML, '  -arr[i] > index');
            // 			arr[i].style.backgroundColor = 'white'
            // 		}
            // 	}
            // }
        }
    }
}
</script>

<style scoped>

</style>