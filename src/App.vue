<template>
    <div class="sample container">
        <div class="row">
            <div class="col-sm">

                <ModalWindow v-bind="modal"
                             v-on:submit="onAdd($event)"
                             v-on:edit="onEdit($event)"
                >
                </ModalWindow>
            </div>
            <div class="col-sm d-flex align-items-center">
                <div class="col-sm-6 text-end">
                    FILTER:
                </div>
                <div class="col-sm-6">
                    <select class="form-select" v-model="selected">
                        <option value="all">All</option>
                        <option value="done">Done</option>
                        <option value="undone">Undone</option>
                        <option value="undone-done">Undone->Done</option>
                        <option value="done-undone">Done->Undone</option>
                    </select>
                </div>
            </div>

            <div class="col-sm">
                <form class="d-flex" v-on:submit.prevent="onSearch($event)">
                    <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search"
                    >
                    <button class="btn btn-outline-success" type="submit">
                        Search
                    </button>
                </form>
            </div>
        </div>
        <hr>
        <span>Выбрано: {{ selected }}</span>
        <hr>
        <br>
        <br>
        <hr>

        <h1>LIST my wish</h1>
        <ul class="list-group">
            <template v-if="selected === ''">
                <p>RENDER TODO</p>
                <TodoItem v-for="(item) in todo" v-bind:prop="item" v-bind:key="item.id"
                          v-on:change="onChange(item.id, $event)" v-on:delete="onDelete(item.id)"
                          v-on:modal="onModal(item)">
                </TodoItem>
            </template>
            <template v-else>
                <p>RENDER FITERED</p>
                <TodoItem v-for="(item) in filtredArr" v-bind:prop="item" v-bind:key="item.id"
                          v-on:change="onChange(item.id, $event)" v-on:delete="onDelete(item.id)"
                          v-on:modal="onModal(item)">
                </TodoItem>
            </template>

        </ul>
    </div>
</template>


<script>
import ModalWindow from './components/modal-window';
import TodoItem from './components/todo-item';

export default {
    name: 'App',
    components: {
        ModalWindow,
        TodoItem,
    },
    data() {
        return {
            todo: [
                {
                    id: 1,
                    name: "JavaScript",
                    descript: "Its a good idea, I am going to read book Devid Flenegan",
                    done: false,
                    star: 3
                },

                {
                    id: 2,
                    name: "Css",
                    descript: "David Soyer CSS3, recently I've bought one",
                    done: true,
                    star: 2
                },
                {
                    id: 3,
                    name: "Html",
                    descript: "Eric Friman Html5",
                    done: false,
                    star: ''
                },
                {
                    id: 4,
                    name: "React",
                    descript: "create the first project via react",
                    done: true,
                    star: ''
                },
            ],
            selected: '',
            modal: ''
        }
    },
    computed: {
        filtredArr: function () {
            console.log('computed done');
            let rez;
            switch (this.selected) {
                case 'done':
                    rez = this.filterArrFun(true)
                    break;
                case 'undone':
                    rez = this.filterArrFun(false)
                    break;
                case 'all':
                    rez = this.todo;
                    break;
                case 'undone-done':
                    rez = this.sortArr('undone-done')
                    break;
                case 'done-undone':
                    rez = this.sortArr('done-undone')
                    break;
            }
            return rez;
        }
    },
    methods: {
        onAdd: function (data) {
            let newData = {
                id: this.todo.length + 1,
                name: data.header,
                descript: data.descript,
                done: false
            }
            this.todo.push(newData)
        },
        onChange: function (id, data) {
            console.log(data.check, '  - CHECK in App.js');
            this.todo.forEach((item) => {
                if (item.id === id) {
                    item.done = data.check
                }
            })
            console.log(' - change');
            this.todo.forEach((item) => {
                console.log(item.done, ' - done');
            })
        },
        sortArr: function (param) {
            if (param === 'undone-done') {
                return this.todo.sort((a, b) => {
                    if (a.done > b.done) {
                        return 1
                    } else {
                        return -1
                    }
                })
            }
            if (param === 'done-undone') {
                return this.todo.sort((a, b) => {
                    if (a.done < b.done) {
                        return 1
                    } else {
                        return -1
                    }
                })
            }
        },
        filterArrFun: function (param) {
            return this.todo.filter((item) => {
                return item.done === param;
            })
        },
        onDelete: function (id) {
            let rez = [];
            this.todo.forEach((item) => {
                if (item.id !== id) {
                    rez.push(item)
                }
            })
            this.todo = rez;
        },
        onEdit: function (data) {
            console.log(data, ' - data');
            let idx = this.todo.findIndex((item) => {
                if (item.id === data.id) {
                    return item
                }
            })
            this.todo[idx].name = data.editHeader
            this.todo[idx].descript = data.editDescript
        },
        onModal: function (item) {
            if (item === 'add') {
                this.modal = {btn: 'Add'};
            } else {
                let index = this.todo.findIndex(i => {
                    return i.id === item.id
                });
                let {id, name, descript} = this.todo[index];
                this.modal = {id: id, name: name, descript: descript, btn: 'Edit'};
            }
        }
    }
}
</script>

<style>

</style>
