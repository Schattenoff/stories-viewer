<script>
export default {
    name: "StoryTimeline",

    props: ['slides', 'currentIndex', 'isPaused'],

    data() {
        return {
            timer: null,
        }
    },



}
</script>

<template>
    <div class="timeline" :class="{'timeline--paused': isPaused}">
        <div v-for="(slide, index) in slides"
             class="timeline__item"
             :class="{
             'timeline__item--past': index < currentIndex,
             'timeline__item--current': index === currentIndex,
             'timeline__item--future': index > currentIndex
             }"
             :style="{width: `calc(100% / ${slides.length})`}"></div>
    </div>
</template>

<style src="./story-timeline.css" scoped>

</style>