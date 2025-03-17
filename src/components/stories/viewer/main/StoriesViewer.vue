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
            swipeMoveAdjusted: false,
            swipeHorizontalChecked: false,
            swipeOffsetX: 0,
            swipeOffsetY: 0,
            swipeOffsetZ: 0,
            isPanning: false,
            currentIndex: 0,
            manager: null,
        }
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
            return {
                'background-image': `linear-gradient(rgba(0, 0, 0, 0.1) 0%, rgba(0, 0, 0, 0.9) 100%), url(${this.bgUrl})`
            };
        },

    },

    mounted() {
        this.manager = new Hammer.Manager(this.$refs.storiesRef, {
            recognizers: [
                [Hammer.Pan],
            ]
        });

        this.manager.on('panstart', (e) => {

            const deltaX = e.deltaX;
            const deltaY = e.deltaY;

            const threshold = 10;

            if (Math.abs(deltaX) > Math.abs(deltaY)) {
                if (Math.abs(deltaX) > threshold) {
                    this.swipeHorizontalChecked = true;
                }
            } else {
                if (Math.abs(deltaY) > threshold) {
                    this.swipeHorizontalChecked = false;
                }
            }

            this.swipeMoveAdjusted = true;
        });

        this.manager.on('panmove', (e) => {
            this.isPanning = true;

            const deltaX = e.deltaX;
            const deltaY = e.deltaY;

            if(this.swipeMoveAdjusted) {

                if(!this.swipeHorizontalChecked) {
                    if (deltaY > 0) {
                        this.swipeOffsetY = Math.min(deltaY / 8, 100);
                    }
                    return;
                }

                let newOffset = (deltaX / this.$refs.storiesRef.offsetWidth) * 100;

                if (this.stories.length === 1) {
                    return;
                } else {
                    if (this.currentIndex === 0) {
                        if(newOffset > 0) {
                            newOffset = Math.max(newOffset, -5);
                            newOffset *= 0.1;
                        } else {
                            newOffset = Math.min(newOffset, 5);
                        }
                    } else if (this.currentIndex === this.stories.length - 1) {
                        if(newOffset < 0) {
                            newOffset = Math.min(newOffset, -5);
                            newOffset *= 0.1;
                        } else {
                            newOffset = Math.max(newOffset, 5);
                        }
                    }
                }

                // Общее ограничение на случай выхода за пределы
                newOffset = Math.max(Math.min(newOffset, 100), -100);

                this.swipeOffsetX = newOffset;
            }
        });

        this.manager.on('panend', (e) => {
            const threshold = 10;

            if(this.swipeOffsetY >= 25) {
                this.onClose();
            }

            if (this.swipeOffsetX < -threshold) {
                this.onNextStory();
            } else if (this.swipeOffsetX > threshold) {
                this.onPrevStory();
            }

            this.swipeOffsetY = 0;
            this.swipeOffsetX = 0;
            this.isPanning = false;
        })

        this.currentIndex = this.stories.findIndex(story => story.id === this.active);
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
    },
}
</script>

<template>
    <div class="stories__viewer">
        <div class="stories__viewerBg" :style="bgStyle"></div>
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
                          translateX(${100 * (index - currentIndex) + swipeOffsetX}%)
                          translateZ(${index === currentIndex ? 0 : -90 + Math.abs(swipeOffsetX)}px)
                          translateY(${swipeOffsetY}%)
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