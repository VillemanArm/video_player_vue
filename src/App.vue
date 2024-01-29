<script>
    import Hls from "hls.js"
    import Controls from "@/components/Controls.vue"
    // import { Play } from 'lucide-vue-next';



    export default {
        components: { Controls },
        data() {
            return {
                videoURL: 'https://devstreaming-cdn.apple.com/videos/streaming/examples/img_bipbop_adv_example_ts/master.m3u8',
                videoCurrentTime: 0,
                videoBufferedTime: 0,
                videoDuration: 0,
                isVideoPlay: false,
                isFullscreen: false,
                isVideoLoading: false,
            }
        },
        methods: {
            showVideoParams() {
                console.log(this.$refs.video.buffered)
            },
            getCurrentTime(time) {
                this.videoCurrentTime = Math.floor(time)                
            },
            getVideoDuration() {
                this.videoDuration = this.$refs.video.duration
            },
            playVideo() {
                this.$refs.video.play()
                this.isVideoPlay = true
            },
            pauseVideo() {
                this.$refs.video.pause()
                this.isVideoPlay = false
            },
            toggleFullScreen() {
                const container = document.querySelector('.video-player');
                const fullscreenApi = container.requestFullscreen
                    || container.webkitRequestFullScreen
                    || container.mozRequestFullScreen
                    || container.msRequestFullscreen;
                if (!document.fullscreenElement) {
                    fullscreenApi.call(container);
                    this.isFullscreen = true
                }
                else {
                    document.exitFullscreen();
                    this.isFullscreen = false
                }                            
            },
            getVideo() {
                try{             
                    const hls = new Hls();
                    hls.loadSource(this.videoURL);
                    hls.attachMedia(this.$refs.video);

                    hls.on(Hls.Events.BUFFER_APPENDED, (event, data) => {
                        if (data.type === 'video') {
                            this.videoBufferedTime = Math.round(data.frag.end)
                        }
                });               
                } catch(error) {
                    console.error(error.message)
                }
            },
            async loadVideo() {
                this.isVideoLoading = true
                await this.getVideo()
                this.isVideoLoading = false
            }

        },
        watch: {},
        mounted() {
            this.loadVideo()
        },
        computed: {},
    }
</script>


<template>

    <div class="video-player">
        <div class="video-player__background"></div>
        <video 
            ref="video"           
            @timeupdate="e => {getCurrentTime(e.target.currentTime)}"
            @canplay="getVideoDuration()"
        ></video>

        <Controls 
            class="video-player__controls"
            :videoBufferedTime="videoBufferedTime"
            :videoDuration="videoDuration"
            :videoCurrentTime="videoCurrentTime"
            :isVideoPlay="isVideoPlay"
            :isFullscreen="isFullscreen"
            :isVideoLoading="isVideoLoading"
            :playVideo="playVideo"
            :pauseVideo="pauseVideo"
            :toggleFullScreen="toggleFullScreen"
        />


    </div>
</template>

<style href="./assets/main.sass" lang="sass"></style>

<style lang="sass">
    @import "./assets/constants.sass"

    .video-player
        width: 600px
        position: relative
        display: flex
    
    .video-player:fullscreen
        background-color: #fff
        width: 100vw

    .video-player__background
        width: 100%
        height: 100%
        position: absolute
        z-index: 0

        background-color: #000

    video
        width: 100%
        position: relative
        z-index: 1          

</style>
