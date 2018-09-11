<template>
    <div class="news">
        <text class="news-title">新闻动态</text>

         <block v-for="item in news" :key="item.id">
        <a :href="'/pages/news-detail/main?id=' + item.id">
            <div class="news-item line">
                <div class="news-item-pic">
                    <image mode="widthFix" :src="item.pic" class="news-item-image" width="" height=""></image>
                </div>
                <div class="news-item-words">
                    <div class="news-item-title"><text>{{item.title}}</text></div>
                    <div class="news-item-content"><text>{{item.content}}</text></div>
                </div>
            </div>
        </a>
        </block>
    </div>
</template>

<script>
import config from '../../config'

export default {

    data () {
        return {
            news: [
                {
                    'id': 10,
                    'pic': 'http://www.jsssha.com/wp-content/uploads/2016/10/1-300x250.jpg',
                    'title': '2016首届新型智慧城市发展高峰论坛在京召开 盛世华安受邀出席',
                    'content': '10月20日，由中国信息协会主办，中国信息界杂志社、中国信息协会信息惠民分会承办的“2016首届新型智慧城市发展高峰'
                },
                {
                    'id': 11,
                    'pic': 'http://www.jsssha.com/wp-content/uploads/2016/10/1-300x250.png',
                    'title': '盛世华安成为我市首届 “港城合伙人”',
                    'content': '10月18日，“港城合伙人”峰会首期开幕式在张家港暨阳湖大酒店隆重召开。现场有上市企业、新三板挂牌企业、创新创业企业代表，全国多家'
                },
                {
                    'id': 12,
                    'pic': 'http://www.jsssha.com/wp-content/uploads/2016/10/DSC_4148-300x250.jpg',
                    'title': '张家港市市长黄戟一行莅临盛世华安调研指导',
                    'content': '9月22日，张家港市市长黄戟一行来到江苏盛世华安智能科技股份有限公司调研指导工作，盛世华安董事长孙浩元、常务副总郭晓桦等公司领导进'
                },
                {
                    'id': 13,
                    'pic':'http://www.jsssha.com/wp-content/uploads/2016/10/%E9%9D%92%E9%93%9C%E5%B3%A1-300x250.jpg',
                    'title': '盛世华安与宁夏青铜峡市达成战略合作',
                    'content': '近日，在张家港市工商联的牵头下，江苏盛世华安智能科技股份有限公司董事长孙浩元等一行来到宁夏青铜峡市进行参观及项目路演，与青铜峡市人'
                },
                {
                    'id': 14,
                    'pic':'http://www.jsssha.com/wp-content/uploads/2016/10/%E6%83%A0%E6%B0%91%E9%80%9A%E6%9C%8D%E5%8A%A1%E7%AB%99-300x250.jpg',
                    'title': '港城首家“盛世惠民通社区便民服务中心”盛大开业',
                    'content': '按2016年张家港市编制的“十三五”规划内容，为持续推进国家级物联网重大应用示范，加快惠民通信息服务平台建设，实现互联网+生活的集'
                },
                {
                    'id': 15,
                    'pic':'http://www.jsssha.com/wp-content/uploads/2016/09/20130604083316_39766-300x250.jpg',
                    'title': '热烈欢迎副市长赵金龙等领导一行莅临盛世华安',
                    'content': '2016年9月2号，张家港市副市长兼公安局局长赵金龙、财政局局长周兵一行到江苏盛世华安智能科技股份有限公司调研指导工作，盛世华安董'
                }
            ]
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
