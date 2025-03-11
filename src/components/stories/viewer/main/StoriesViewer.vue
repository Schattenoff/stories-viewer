<script>
import Story from "@/components/stories/viewer/story/main/Story.vue";
import Hammer from "hammerjs";

export default {
    name: "StoriesViewer",

    components: {
        Story,
    },

    props: ['stories', 'active'],

    data() {
        return {
            bgUrl: '',
            swipeOffset: 0,
            panY: 0,
            isPanning: false,
            currentIndex: 0,
            manager: null,
        }
    },

    mounted() {
        this.manager = new Hammer.Manager(this.$refs.storiesRef, {
            recognizers: [
                [Hammer.Pan],
            ]
        });

        this.manager.on('pan', (e) => {
            this.isPanning = true;

            const deltaX = e.deltaX;
            const deltaY = e.deltaY;

            if(deltaY >= 5 && (deltaX < 5 && deltaX > -5)) {
                this.panY = Math.min(deltaY / 5, 100); // Плавное смещение вниз
                console.log(this.panY);
                return;
            }

            let newOffset = (deltaX / this.$refs.storiesRef.offsetWidth) * 100;

            if (this.currentIndex === 0) {
                newOffset = Math.min(newOffset, 5);
            } else if (this.currentIndex === this.stories.length - 1) {
                newOffset = Math.max(newOffset, -5);
            }

            newOffset = Math.max(Math.min(newOffset, 100), -100);

            this.swipeOffset = newOffset;
        });

        this.manager.on('panend', (e) => {
            const threshold = 5;

            if(this.panY >= 30) {
                this.onClose();
            }

            if (this.swipeOffset < -threshold) {
                this.onNextStory();
            } else if (this.swipeOffset > threshold) {
                this.onPrevStory();
            }

            this.panY = 0;
            this.swipeOffset = 0;
            this.isPanning = false;
        })

        this.currentIndex = this.stories.findIndex(story => story.id === this.active);
    },

    computed: {
        currentStory() {
            return this.stories[this.currentIndex];
        },

        isDisabledNext() {
            return this.currentIndex < this.stories.length - 1;
        },

        isDisabledPrev() {
            return this.currentIndex > 0;
        },

        bgStyle() {
            return `linear-gradient(rgba(0, 0, 0, 0.1) 0%, rgba(0, 0, 0, 0.9) 100%), url(${this.bgUrl})`;
        }
    },

    methods: {
        onClose() {
            this.$emit('close');
        },

        onNextStory() {
            if(this.isDisabledNext) {
                this.currentIndex = this.currentIndex + 1;
            } else {
                this.onClose();
            }
        },

        onPrevStory() {
            if(this.isDisabledPrev) {
                this.currentIndex = this.currentIndex - 1;
            } else {
                this.onClose();
            }
        },

        setBgUrl(url) {
            this.bgUrl = url;
        }
    }
}
</script>

<template>
    <div class="stories__viewer">
        <div class="stories__viewerBg" :style="{'background-image': bgStyle}"></div>
        <div class="stories__wrapper">
            <div class="stories__arrow stories__arrow--left"
                 :class="{'stories__arrow--disabled': !isDisabledPrev}"
                 @click="isDisabledPrev && onPrevStory()"></div>
            <div class="stories__container" ref="storiesRef">
                <Story v-for="(story, index) in stories"
                       class="slide-animation"
                       :class="{
                         'prev': index < currentIndex,
                         'next': index > currentIndex,
                         'no-transition': isPanning
                       }"
                       :style="{
                        transform: `
                          translateX(${100 * (index - currentIndex) + swipeOffset}%)
                          ${panY ? `translateY(${panY}%)` : ''}
                        `}"
                       :key="story.id"
                       :story="story"
                       :is-panning="isPanning"
                       :active="story.id === currentStory.id"
                       @close="onClose"
                       @bg="setBgUrl" />
            </div>
            <div class="stories__arrow stories__arrow--right"
                 :class="{'stories__arrow--disabled': !isDisabledNext}"
                 @click="isDisabledNext && onNextStory()"></div>
        </div>
    </div>
</template>

<style src="./stories-viewer.css" scoped>

</style>