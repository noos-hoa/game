<template>
    <div>
        <div
            v-show="isShowModal"
            class="modal fade"
            :class="{'show': isShowModal}"
            tabindex="-1"
            role="dialog"
            aria-hidden="true"
        >
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLongTitle">The game is end</h5>
                        <button
                            type="button"
                            class="close"
                            data-dismiss="modal"
                            aria-label="Close"
                            @click="onCloseModal"
                        >
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        {{modalText}}
                    </div>
                    <div class="modal-footer">
                        <button
                            type="button"
                            class="btn btn-secondary"
                            data-dismiss="modal"
                            @click="onCloseModal"
                        >Close
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <div
            class="modal-backdrop fade"
            :class="{'show': isShowModal}"
            v-show="isShowModal"
        ></div>
    </div>
</template>

<script>
    export default {
        name: "PopUp",
        props: {
            isShowModal: {
                type: Boolean,
                default: false
            },
            modalText: String
        },
        methods: {
            onCloseModal() {
                this.$emit('closePopUp')
            }
        }
    }
</script>

<style scoped lang="scss">
    .modal {
        &.show {
            display: block;
        }
    }
</style>