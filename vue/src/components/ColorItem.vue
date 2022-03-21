<template>
    <div
        class="card m-1"
        :class="{'border border-primary': selected}"
        @click.stop="$emit('on-selected')"
    >
        <div class="card-body d-flex flex-column justify-content-end" :style="style">
            <button class="btn btn-sm" @click.stop="remove">x</button>
            <input type="text" v-model="color">
        </div>
    </div>
</template>

<script>
    export default {
        name: 'ColorItem',
        props: {
            item: {
                type: Object,
                required: true,
            },
            selected: {
                type: Boolean,
                required: true,
            },
        },
        data: ({item}) => ({
            color: item.color,
        }),
        methods: {
            remove() {
                this.$emit('on-remove', this.item);
            },
        },
        computed: {
            style() {
                return {
                    'background-color': this.color,
                };
            },
        },
        watch: {
            color(val) {
                this.$emit('on-update-color', val);
            },
            item: {
                handler(val) {
                    this.color = val.color;
                },
                deep: true,
            },
        },
    };
</script>

<style scoped>
    .card {
        width: 140px;
        height: 150px;
    }

    .card .card-body button {
        position: absolute;
        top: 5px;
        right: 5px;
    }

    .border {
        border-width: 3px !important;
    }
</style>
