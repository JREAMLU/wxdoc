<template>
    <div class="page">
        <div class="page__bd">
            <div class="section section_gap">
                <map style="width: 100%; height: 300px;" :latitude="latitude" :longitude="longitude" :markers="markers" :scale="scale"></map>
                <div class="contact">
                    <wxParse :content="content" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import config from '../../config'
import wxParse from 'mpvue-wxparse';

export default {

    data () {
        return {
            title: '',
            latitude: 0,
            longitude: 0,
            scale: 0,
            markers: [],
            content: '',
        }
    },

    components: {
        wxParse
    },

    created () {
    },

    onLoad() {
        this.contact();
    },

    methods: {
        contact() {
            var that = this;
            wx.request({
                url: config.API_URL.CONTACT,
                method: 'POST',
                data: {},
                header: {
                    'content-type': 'application/json',
                },
                success: function(res) {
                   that.title = res.data.data.title;
                   that.latitude = res.data.data.latitude;
                   that.longitude = res.data.data.longitude;
                   that.scale = res.data.data.scale;
                   that.markers = res.data.data.markers;
                   that.content = res.data.data.content;
                },
            });
        },
    },

    watch: {
    }
}
</script>

<style scoped>
@import "../../../static/css/app.css";
</style>
