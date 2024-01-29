<script>
    import { Pause, Expand, Shrink } from 'lucide-vue-next';
    import  Play from './Play.vue';
    
    //TODO: сделать управляемый ползунок
    //TODO: сделать скрытие управления в fullscreen
    //TODO: сделать анимацию загрузки
    //TODO: отредактировать иконку паузы

    export default {
        components: { Play, Pause, Expand, Shrink },
        props: {
            playVideo: {
                type: Function,
                required: true,
            },
            pauseVideo: {
                type: Function,
                required: true,
            },
            videoCurrentTime: {
                type: Number,
                required: true,
            },
            videoDuration: {
                type: Number,
                required: true,
            },
            videoBufferedTime: {
                type: Number,
                required: true,
            },
            isVideoPlay: {
                type: Boolean,
                required: true,
            },
            isFullscreen: {
                type: Boolean,
                required: true,
            },
            toggleFullScreen: {
                type: Function,
                required: true,
            },
        },
        data() {
            return {
                

            }
        },
        methods: {
            toggleControls() {
                this.$refs.controls.style.opacity = '1'
                let mouseTimer
                clearTimeout(mouseTimer);
                mouseTimer = setTimeout(() => {
                    if (this.isVideoPlay && this.isFullscreen) this.$refs.controls.style.opacity = '0'
                }, 2000)                
            },
            formatTime(seconds) {
                const formatedSeconds = ('0' + seconds % 60).slice(-2)
                const formatedMinutes = ('0' + Math.floor(seconds / 60) % 60).slice(-2)
                const formatedHours = Math.floor(seconds / 3600)
                return `${formatedHours ? formatedHours + ':' : ''}${formatedMinutes}:${formatedSeconds}`
            },
            hideControls() {
                if (this.isVideoPlay) this.$refs.controls.style.opacity = '0'
            },
        },
        computed: {
            formattedDuration() {
                return this.formatTime(this.videoDuration)
            },
            formattedCurrentTime() {
                return this.formatTime(this.videoCurrentTime)
            },
            formattedBufferedTime() {
                return this.formatTime(this.videoBufferedTime)
            },
        }
    }
</script>

<template>
    <div 
        ref="controls" 
        class="controls__wrapper" 
        @mousemove="toggleControls()"
        @mouseleave="hideControls()"
        
        >
        <div class="controls__main-button" ref="mainButton">
            <Pause v-if="isVideoPlay" @click="pauseVideo"/>
            <Play v-else class="controls__main-button-play" @click="playVideo"/>
        </div>

        <div class="controls__bottom-panel">
            <div class="controls__progress-bar">
                <div class="controls__full-line">
                    <div 
                        class="controls__buffered-time"
                        :style="{
                            'width': `${(videoBufferedTime / videoDuration) * 100}%`
                        }"
                    ></div>
                    <div 
                        class="controls__current-time"
                        :style="{
                            'width': `${(videoCurrentTime / videoDuration) * 100}%`
                        }"
                    ></div>
                    
                </div>
            </div>
            <div class="controls__bottom-panel_bottom">
                <div class="controls__play">
                    <Pause v-if="isVideoPlay" @click="pauseVideo"/>
                    <Play v-else class="controls__main-button-play" @click="playVideo"/>
                </div>
                <div class="controls__play-statistics">                
                    <div class="video-time">Current time: {{ formattedCurrentTime }}</div>
                    <div class="video-buffered-time">Buffered time: {{ formattedBufferedTime }}</div>
                    <div class="video-duration">Duration: {{ formattedDuration }}</div>
                </div>
                <div class="controls__fullscreen" @click="toggleFullScreen">
                    <Shrink v-if="isFullscreen"/>
                    <Expand v-else/>
                </div>
            </div>
        </div>

        <!-- <Play class="play"/> -->

    </div>
</template>

<style scoped lang="sass">
    @import "../assets/constants.sass"

    .controls__wrapper
        position: absolute
        width: 100%
        height: 100%
        bottom: 0
        display: grid
        z-index: 2
        align-items: center

        font-family: 'Ubuntu', sans-serif

        color: #fff
        transition: all 0.4s ease
        

    .controls__main-button
        align-self: center
        justify-self: center
        width: 32px
        height: 32px
        display: flex
        justify-content: center
        align-items: center

        border-radius: 50%
        background-color: $primary-color
        cursor: pointer
        transition: all 0.4s ease

        &:hover
            transform: scale(1.2)

    .controls__main-button-play
        margin-left: 2px
    .controls__bottom-panel
        position: absolute
        bottom: 0
        width: 100%
        display: flex
        flex-direction: column
        justify-content: space-between
        padding: 2px 4px
        
        background: linear-gradient(0deg, rgba(0,0,0,1) 0%, rgba(255,255,255,0) 100%)
        transition: all 0.4s ease

    .controls__progress-bar
        width: 100%
        height: 16px
        display: flex
        align-items: center
        justify-content: center

    .controls__full-line
        position: relative
        width: 90%
        height: 4px

        background-color: rgba(255, 255, 255, 0.6)

    .controls__buffered-time
        position: absolute
        height: 100%
        top: 0
        z-index: 2

        background-color: #fff

    .controls__current-time
        position: absolute
        height: 100%
        top: 0
        z-index: 3

        background-color: $primary-color


    .controls__bottom-panel_bottom
        display: flex
        justify-content: space-between
        align-items: center

    .controls__play-statistics
        display: flex
        gap: 10px
        margin-bottom: 2px

        font-size: 14px

    .controls__play:hover svg
        stroke: $primary-color
        cursor: pointer

    .controls__fullscreen:hover svg
        stroke: $primary-color
        cursor: pointer

    svg
        stroke: #fff
    
</style>
