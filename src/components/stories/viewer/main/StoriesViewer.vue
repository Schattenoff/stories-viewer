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
            rotateY: 0,
            swipeOffsetX: 0,
            swipeOffsetY: 0,
            isPanning: false,
            isAnimating: false,
            isTransitionEnabled: false,
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
                        this.swipeOffsetY = Math.min(deltaY / 5, 100);
                    }
                    return;
                }

                // let newOffset = (deltaX / this.$refs.storiesRef.offsetWidth) * 100;
                //
                // if (this.stories.length === 1) {
                //     // Если только одна сторис, ограничиваем смещение в обе стороны
                //     newOffset = Math.max(Math.min(newOffset, 5), -5);
                // } else {
                //     if (this.currentIndex === 0) {
                //         // Если первая сторис, ограничиваем смещение вправо
                //         newOffset = Math.min(newOffset, 5);
                //     } else if (this.currentIndex === this.stories.length - 1) {
                //         // Если последняя сторис, ограничиваем смещение влево
                //         newOffset = Math.max(newOffset, -5);
                //     }
                // }
                //
                // // Общее ограничение на случай выхода за пределы
                // newOffset = Math.max(Math.min(newOffset, 100), -100);
                //
                // this.swipeOffsetX = newOffset;

                let newRotateY = (deltaX / this.$refs.storiesRef.offsetWidth) * 90; // Максимальный угол 30 градусов

                if (this.stories.length === 1) {
                    // Если только одна сторис, ограничиваем вращение в обе стороны
                    newRotateY = Math.max(Math.min(newRotateY, 15), -15); // Ограничение до 15 градусов
                } else {
                    if (this.currentIndex === 0) {
                        // Если первая сторис, ограничиваем вращение вправо
                        newRotateY = Math.min(newRotateY, 15);
                    } else if (this.currentIndex === this.stories.length - 1) {
                        // Если последняя сторис, ограничиваем вращение влево
                        newRotateY = Math.max(newRotateY, -15);
                    }
                }

                // Общее ограничение на случай выхода за пределы
                newRotateY = Math.max(Math.min(newRotateY, 90), -90);

                this.rotateY = newRotateY;
            }
        });

        this.manager.on('panend', (e) => {
            const threshold = 15;

            if(this.panY >= 30) {
                this.onClose();
            }

            if (this.rotateY < -threshold) {
                this.onNextStory();
            } else if (this.rotateY > threshold) {
                this.onPrevStory();
            }

            this.swipeOffsetY = 0;
            this.isPanning = false;
        })

        this.currentIndex = this.stories.findIndex(story => story.id === this.active);
    },

    methods: {
        onClose() {
            this.$emit('close');
        },

        onNextStory() {
            if(this.isAnimating) return;

            if(this.isDisabledNext) {
                this.isAnimating = true;

                this.rotateY = -90;

                setTimeout(() => {
                    this.rotateY = 0;
                    this.currentIndex = this.currentIndex + 1;
                    this.isAnimating = false;
                }, 300);


            } else {
                this.onClose();
            }
        },

        onPrevStory() {
            if(this.isAnimating) return;

            if(this.isDisabledPrev) {
                this.isAnimating = true;

                this.rotateY = 90;

                setTimeout(() => {
                    this.rotateY = 0;
                    this.currentIndex = this.currentIndex - 1;
                    this.isAnimating = false;
                }, 300);

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
            <div class="stories__container"
                 ref="storiesRef"
                 :class="{'no-transition': isPanning}"
                 :style="{
                   transform: `
                     rotateY(${rotateY}deg)
                   `,
                 }">
                <Story v-for="(story, index) in stories"
                       class="slide-animation"
                       :class="{
                         'current': index === currentIndex,
                         'prev': index === currentIndex - 1,
                         'next': index === currentIndex + 1,
                         'no-transition': isPanning
                       }"
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