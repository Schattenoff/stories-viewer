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

    props: ['story', 'active', 'isPanning'],

    data() {
        return {
            isPaused: false,
            currentIndex: 0,
            timer: null,
            duration: 15000,
            startTime: 0,
            elapsedTime: 0,
            manager: null,
        }
    },

    computed: {
        slides() {
            return this.story.slides;
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
            this.$emit('bg', this.slides[this.currentIndex].src);
            this.startAutoPlay();
        }

        this.initHammerManager();
    },

    watch: {
        active(newVal) {
            if (newVal) {
                this.$emit('bg', this.slides[this.currentIndex].src);
                this.startAutoPlay();
            } else {
                this.stopAutoPlay();
            }
        },

        isAnimating(newVal) {
            if (newVal && this.active) {
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
                ]
            });

            this.manager.on('tap', (e) => {
                // если это кнопка закрыть, то не идем дальше
                if(e.target.classList[0].includes('close')) {
                    return;
                }

                // нажали на какое-то действие
                if(e.target.classList[0].includes('action')) {
                    console.log('action');
                    return;
                }

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

        },

        onPrevSlide() {
            if(!this.isDisabledPrev) {
                this.$parent.onPrevStory();
            } else {
                this.currentIndex = this.currentIndex - 1;
            }

            this.$emit('bg', this.slides[this.currentIndex].src);
            this.resetElapsedTime();
            this.stopAutoPlay();
            this.startAutoPlay();
        },

        onNextSlide() {
            if(!this.isDisabledNext) {
                this.$parent.onNextStory();
            } else {
                this.currentIndex = this.currentIndex + 1;
            }

            this.$emit('bg', this.slides[this.currentIndex].src);
            this.resetElapsedTime();
            this.stopAutoPlay();
            this.startAutoPlay();
        },

        onClose() {
            this.$emit('close');
        },

        startAutoPlay(remainingDuration) {
            this.startTime = Date.now();
            this.timer = setTimeout(() => {
                this.onNextSlide();
            }, remainingDuration ? remainingDuration : this.duration);
        },

        stopAutoPlay() {
            clearTimeout(this.timer);
        },

        pauseStory() {
            this.isPaused = true;
            this.stopAutoPlay();
            this.elapsedTime += Date.now() - this.startTime;
        },

        playStory() {
            const remainingDuration = this.duration - this.elapsedTime;
            this.isPaused = false;
            this.startAutoPlay(remainingDuration);
        },

        resetElapsedTime() {
            this.elapsedTime = 0;
        }
    }
}
</script>

<template>
    <div class="story"
         :class="{'story--active': active}"
         ref="storyRef">
        <div class="story__close" @click="onClose()"></div>
        <StoryTimeline :slides="slides"
                       :current-index="active && currentIndex"
                       :duration="duration"
                       :is-panning="isPanning"
                       :is-paused="isPaused" />
        <StoryFrame :frame-src="currentSlide.src" />
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