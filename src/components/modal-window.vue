<template>
    <div class="col-sm" v-on:keyup.escape="onModal">
        <button type="button" class="btn btn-primary" v-on:click="onModal()">
            Add new wish
        </button>
        <div class="modal fade"
             v-bind:class="hiden ? '' : 'show'"
             id="exampleModal"
             tabindex="-1"
             v-on:click="onClose"
        >
            <div class="modal-dialog" v-on:click.stop="">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title"
                            v-if="btn === 'Add'"
                        >
                            Write down your wish
                        </h5>

                        <h5 v-else class="modal-title"
                        >
                            This is your wish
                        </h5>
                        <button type="button"
                                class="btn-close"
                                v-on:click="onClose"
                        ></button>
                    </div>
                    <div class="modal-body">
                        <form>
                            <div class="mb-3">
                                <label class="col-form-label"
                                >
                                    Heading:
                                </label>
                                <input v-if="btn === 'Add'"
                                       type="text"
                                       class="form-control test"
                                       v-model="newHeader"
                                >
                                <input v-else
                                       type="text"
                                       class="form-control notest"
                                       v-bind:value="editHeader"
                                       v-bind:disabled="disabled"
                                       v-on:change="onChange($event, 'name')"
                                >
                            </div>
                            <div class="mb-3">
                                <label
                                        class="col-form-label"
                                >
                                    Describtion:
                                </label>
                                <textarea class="form-control"
                                          v-if="btn === 'Add'"
                                          v-model="newDescript"
                                          name="message-text"
                                ></textarea>
                                <textarea class="form-control"
                                          name="message-text"
                                          v-else
                                          v-bind:value="editDescript"
                                          v-bind:disabled="disabled"
                                          v-on:change="onChange($event, 'descript')"
                                ></textarea>

                            </div>
                        </form>
                    </div>
                    <div class="modal-footer">
                        <button type="button"
                                class="btn btn-secondary"
                                v-on:click="onClose"
                        >
                            Close
                        </button>
                        <button v-if="btn === 'Add'" type="button"
                                class="btn btn-primary"
                                data-bs-dismiss="modal"
                                v-on:click="onAdd()"
                        >
                            {{ btn }}
                        </button>
                        <button v-else type="button"
                                class="btn btn-primary"
                                v-bind:data-bs-dismiss="modalForBtn"
                                v-on:click="onEdit()"
                        >
                            {{ btn }}
                        </button>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import {bus} from '../main.js'

export default {
    props: {
        id: {
            type: Number,
        },
        name: {
            type: String,
            default: ''
        },
        descript: {
            type: String,
            default: ''
        }
    },
    data() {
        return {
            newHeader: '',
            newDescript: '',
            editHeader: "",
            editDescript: "",
            editId: "",
            disabled: "",
            modalForBtn: '',
            display: '',
            btn: 'Add',
            modal: '',
            hiden: true,

        }
    },
    created() {
        bus.$on('editDisabled', data => {
            this.editHeader = data.data.name;
            this.editDescript = data.data.descript;
            this.btn = data.btn;
            this.onModal();
        })
    },
    methods: {
        onModal() {
            this.hiden = !this.hiden;
            if(this.btn === 'Edit') {
                this.disabled = true;
            }
        },
        onClose() {
            this.hiden = !this.hiden;
            this.editHeader = "";
            this.editDescript = "";
        },
        onChange(event, param) {
            if (param === 'name') {
                this.editHeader = event.target.value;

            }
            if (param === 'descript') {
                this.editDescript = event.target.value;

            }
        },
        onAdd() {
            if (this.newHeader !== '' && this.newDescript !== '') {
                this.$emit('submit', {
                    header: this.newHeader,
                    descript: this.newDescript
                });
                this.onModal();
            }
            this.newHeader = '';
            this.newDescript = '';

        },
        onEdit() {
            if (this.btn === 'Edit') {
                this.disabled = false;
                this.btn = 'Save'
                this.modalForBtn = 'modal'
                return
            }
            if (this.btn === 'Save') {
                if (this.editHeader !== '' || this.editDescript !== '') {
                    this.$emit('edit', {
                        id: this.id,
                        editHeader: this.editHeader,
                        editDescript: this.editDescript
                    })
                }
                this.onModal();
                this.editHeader = '';
                this.editDescript = '';

            }
        }
    }
}
</script>

<style scoped>
.show {
    display: block;
    background: rgba(158, 158, 158, 0.8);
}
</style>