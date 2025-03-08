<script>
import Hammer from 'hammerjs';
import StoryFrame from "@/components/stories/viewer/story/frame/StoryFrame.vue";
import StoryTimeline from "@/components/stories/viewer/story/timeline/StoryTimeline.vue";
import StoryAction from "@/components/stories/viewer/story/action/StoryAction.vue";

export default {
    name: "Story",

    components: {
        StoryAction,
        StoryFrame,
        StoryTimeline
    },

    props: ['story', 'active', 'storyCurrentIndex'],

    data() {
        return {
            isPaused: false,
            currentIndex: 0,
            timer: null,
            duration: 1900,
            startTime: 0,
            elapsedTime: 0,
            manager: null,
        }
    },

    computed: {
        slides() {
            return this.story.images;
        },

        currentSlide() {
            return this.slides[this.currentIndex];
        },

        isDisabledNext() {
            return this.currentIndex < this.slides.length - 1;
        },

        isDisabledPrev() {
            return this.currentIndex > 0;
        },
    },

    mounted() {
        if(this.active) {
            this.startAutoPlay();
        }

        this.initHammerManager();
    },

    watch: {
        active(newVal) {
            if (newVal) {
                this.startAutoPlay();
            } else {
                this.stopAutoPlay();
            }
        }
    },

    beforeUnmount() {
        this.stopAutoPlay();
    },

    methods: {
        initHammerManager() {
            this.manager = new Hammer.Manager(this.$refs.storyRef, {
                recognizers: [
                    [Hammer.Tap],
                    [Hammer.Press],
                    [Hammer.Swipe],
                ]
            });

            this.manager.on('tap', (e) => {
                if(e.target.classList[0] === 'story__navRight') {
                    this.onNextSlide();
                } else {
                    this.onPrevSlide();
                }
            });

            this.manager.on('press', () => {
                this.pauseStory();
            });

            this.manager.on('pressup', () => {
                this.playStory();
            });

            this.manager.on('swiperight', (e) => {
                console.log(e);
                this.$parent.onPrevStory();

                this.stopAutoPlay();
                this.duration = 15000;
                this.startAutoPlay();
            });

            this.manager.on('swipeleft', () => {
                this.$parent.onNextStory();

                this.stopAutoPlay();
                this.duration = 1900;
                this.startAutoPlay();
            });

        },

        onPrevSlide() {
            if(!this.isDisabledPrev) {
                this.$parent.onPrevStory();
            } else {
                this.currentIndex = this.currentIndex - 1;
            }

            this.stopAutoPlay();
            this.duration = 1900;
            this.startAutoPlay();
        },

        onNextSlide() {
            if(!this.isDisabledNext) {
                this.$parent.onNextStory();
            } else {
                this.currentIndex = this.currentIndex + 1;
            }

            this.stopAutoPlay();
            this.duration = 1900;
            this.startAutoPlay();
        },

        startAutoPlay() {
            this.startTime = Date.now();
            this.timer = setTimeout(() => {
                this.onNextSlide();
            }, this.duration);
        },

        stopAutoPlay() {
            clearTimeout(this.timer);
        },

        pauseStory() {
            this.isPaused = true;
            this.stopAutoPlay();
            this.elapsedTime = Date.now() - this.startTime;
        },

        playStory() {
            this.duration = this.duration - this.elapsedTime;
            this.isPaused = false;
            this.startAutoPlay();
        },
    }
}
</script>

<template>
    <div class="story"
         :class="{'story--active': active}"
         ref="storyRef">
        <StoryTimeline :slides="slides"
                       :current-index="active && currentIndex"
                       :duration="duration"
                       :is-paused="isPaused" />
        <StoryFrame :frame-url="currentSlide.src" />
        <div class="story__nav">
            <div class="story__navLeft"></div>
            <div class="story__navRight"></div>
        </div>
        <StoryAction v-for="(action, i) in currentSlide.actions"
                     :key="i"
                     :action="action" />
    </div>
</template>

<style src="./story.css" scoped>

</style>