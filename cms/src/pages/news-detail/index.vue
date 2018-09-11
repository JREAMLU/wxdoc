<template>
    <div class="page">
        <div class="page__hd">
            <text class="page__title">{{title}}</text>
            <text class="page__desc"></text>
        </div>

        <div class="news-details-content wxParse">
        </div>
    </div>
</template>

<script>
import config from '../../config';

export default {

    data () {
        return {
            title: '',
        }
    },

    components: {
    },

    created () {
    },

    onLoad (options) {
        this.title = 'abc';
    },

    methods: {
    },

    watch: {
    }
}
</script>

<style scoped>
@import "../../../static/css/app.css";
</style>
