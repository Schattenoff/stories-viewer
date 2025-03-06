<script>
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

    props: ['story', 'active'],

    data() {
        return {
            isPaused: false,
            currentIndex: 0,
            timer: null,
            duration: 1900,
            startTime: 0,
            elapsedTime: 0,
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

        goToSlide() {
            this.onNextSlide();
        },

        startAutoPlay() {
            this.startTime = Date.now();
            this.timer = setTimeout(() => {
                this.goToSlide();
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
            this.startAutoPlay(this.duration);
        }
    }
}
</script>

<template>
    <div class="story">
        <StoryTimeline :slides="slides" :current-index="currentIndex" :is-paused="isPaused" />
        <StoryFrame :frame-url="currentSlide.storyImg" />
        <div class="story__nav">
            <div class="story__navLeft" @click="onPrevSlide()"></div>
            <div class="story__navRight" @click="onNextSlide()"></div>
        </div>
        <StoryAction v-for="(action, i) in currentSlide.actions"
                     :key="i"
                     :action="action" />
    </div>
</template>

<style src="./story.css" scoped>

</style>