<template>
    <div id="app">
        <div class="container d-flex flex-row mt-5">
            <div class="card w-100">
                <div class="card-body d-flex flex-wrap">
                    <button class="btn btn-sm" :disabled="lists[0].length >= 6" @click="addItems(0)">+</button>

                    <ColorItem
                        v-for="(item, index) in lists[0]"
                        :key="index"
                        :item="item"
                        :selected="hasSelectedItem && selectedItem.listIndex === 0 && selectedItem.itemIndex === index"
                        @on-update-color="item.color = $event"
                        @on-remove="removeItem(0, index)"
                        @on-selected="selectItem(0, index)"
                    ></ColorItem>
                </div>
            </div>

            <div class="d-flex flex-column justify-content-center mx-3">
                <button class="btn btn-primary mb-2" :disabled="!canMove" @click="moveSelected">Move</button>
                <button class="btn btn-primary mb-2" :disabled="!hasSelectedItem" @click="removeSelected">Delete</button>
                <button class="btn btn-primary mb-2" :disabled="!canMove" @click="copySelected">Copy</button>
                <button class="btn btn-primary" :disabled="!canMove" @click="copySelectedReference">Reference</button>
            </div>

            <div class="card w-100">
                <div class="card-body d-flex flex-wrap">
                    <button class="btn btn-sm" :disabled="lists[1].length >= 6" @click="addItems(1)">+</button>

                    <ColorItem
                        v-for="(item, index) in lists[1]"
                        :key="index"
                        :item="item"
                        :selected="hasSelectedItem && selectedItem.listIndex === 1 && selectedItem.itemIndex === index"
                        @on-update-color="item.color = $event"
                        @on-remove="removeItem(1, index)"
                        @on-selected="selectItem(1, index)"
                    ></ColorItem>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import ColorItem from '@/components/ColorItem';

    export default {
        name: 'App',
        components: {
            ColorItem,
        },
        data: () => ({
            lists: [
                [
                    {
                        color: 'blue',
                    },
                ],
                [],
            ],
            selectedItem: null,
        }),
        methods: {
            moveSelected() {
                this.addItems(this.oppositeListIndex, this.lists[this.selectedItem.listIndex][this.selectedItem.itemIndex]);
                this.removeSelected();
            },
            removeSelected() {
                this.lists[this.selectedItem.listIndex].splice(this.selectedItem.itemIndex, 1);
                this.selectedItem = null;
            },
            copySelected() {
                this.addItems(this.oppositeListIndex, {...this.lists[this.selectedItem.listIndex][this.selectedItem.itemIndex]});
                this.selectedItem = null;
            },
            copySelectedReference() {
                this.addItems(this.oppositeListIndex, this.lists[this.selectedItem.listIndex][this.selectedItem.itemIndex]);
                this.selectedItem = null;
            },
            addItems(listIndex, item = null) {
                if (this.lists[listIndex].length < 6) {
                    if (!item) {
                        item = {
                            color: 'brown',
                        };
                    }

                    this.lists[listIndex].push(item);
                }
            },
            selectItem(listIndex, itemIndex) {
                this.selectedItem = {
                    listIndex,
                    itemIndex,
                };
            },
            removeItem(listIndex, itemIndex) {
                this.selectItem(listIndex, itemIndex);
                this.removeSelected();
            },
        },
        computed: {
            oppositeListIndex() {
                if (this.hasSelectedItem) {
                    return this.selectedItem.listIndex !== 0 ? 0 : 1;
                }

                return null;
            },
            hasSelectedItem() {
                return !!this.selectedItem;
            },
            canMove() {
                return this.hasSelectedItem && this.lists[this.oppositeListIndex].length < 6;
            },
        },
    };
</script>

<style scoped>
    .card .card-body button {
        position: absolute;
        top: -16px;
        right: -12px;
    }
</style>
