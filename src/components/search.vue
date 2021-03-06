<template>
    <div>
        <b-modal id="modal-1" title="BootstrapVue">
            <p class="my-4">Hello from modal!</p>
        </b-modal>
    </div>
</template>

<script>
export default {
    name: 'Search',
    methods: {
        modalShow() {
            this.$bvModal.show('modal-1')
        }
    }
}
</script>

<style scoped>

</style>