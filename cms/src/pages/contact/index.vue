<template>
    <div class="page">
        <div class="page__bd">
            <div class="section section_gap">
                <map style="width: 100%; height: 300px;" :latitude="latitude" :longitude="longitude" :markers="markers" :scale="scale"></map>
                <div class="contact">
                    <text>
                        股票：838280 <br/>
                        电话：0512-58161723 <br/>
                        传真：0512-58161711 <br/>
                        地址：江苏省苏州市张家港市滨河路2号 <br/> 
                        华东国际大厦5F
                    </text>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import config from '../../config'

export default {

    data () {
        return {
            title: '联系我们',
            latitude: 31.838403,
            longitude: 120.528958,
            scale: 16,
            markers: [{
              latitude: 31.838403,
              longitude: 120.528958,
              name: '华东国际大厦5F'
            }]
        }
    },

    components: {
    },

    created () {
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
