<template>
    <transition name="fade">
        <div class="searchPageWrapper">
            <platform-tabs @onChange="onChange"></platform-tabs>
            <div v-if="!innerMusicList.length" class="no-data">
                暂无数据
            </div>
            <music-list height="580px" :musicList="innerMusicList" v-else/>

        </div>
    </transition>
</template>

<script>
    import MusicList from '../../components/common/musicList/musicList';
    import PlatformTabs from './platformTabs';
    import { mapGetters } from 'vuex';

    export default {
        name:'search-page',
        components:{
            MusicList,
            PlatformTabs
        },
        computed:{
            ...mapGetters({
                innerMusicList:'getMusicList',
                token:'token'
            })
        },
        data(){
            return {

            }
        },
        methods:{
            onChange(platform) {
                this.$store.commit('SET_PLATFORM', platform)
                //重新搜索
                const page = 1
                const token = this.token
                this.$store.dispatch('search', {page, token} ).then(res => {
                }).catch(error => {
                    console.log('搜索失败！错误原因：', error)
                })
            }
        }
    }
</script>

<style lang="stylus">
    .searchPageWrapper {
        overflow: hidden;
        .no-data {
            width: 910px;
            height: 580px;
            background-color: #fff;
            text-align: center;
            line-height: 580px;
            font-size: 15px;
            font-weight: bold;
        }
    }
</style>