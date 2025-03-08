<script>
import Story from "@/components/stories/viewer/story/main/Story.vue";

export default {
    name: "StoriesViewer",

    components: {
        Story,
    },

    props: ['stories', 'active'],

    data() {
        return {
            currentIndex: 0,
        }
    },

    mounted() {
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
        }
    }
}
</script>

<template>
    <div class="stories__viewer">
        <div class="stories__close" @click="onClose">close</div>
        <div class="stories__wrapper">
            <div class="stories__arrow"
                 :class="{'stories__arrow--disabled': !isDisabledPrev}"
                 @click="onPrevStory()">left</div>
            <div class="stories__container">
                <Story v-for="(story, index) in stories"
                       class="slide-animation"
                       :class="{
                         'prev': index < currentIndex,
                         'next': index > currentIndex
                       }"
                       :key="story.id"
                       :story="story"
                       :story-current-index="currentIndex"
                       :active="story.id === currentStory.id" />
            </div>
            <div class="stories__arrow"
                 :class="{'stories__arrow--disabled': !isDisabledNext}"
                 @click="onNextStory()">right</div>
        </div>
    </div>
</template>

<style src="./stories-viewer.css" scoped>

</style>