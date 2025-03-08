<script>
export default {
    name: "StoryTimeline",

    props: ['slides', 'currentIndex', 'duration', 'isPaused'],

    computed: {
        durationStyle() {
            return `${(this.duration + 100) / 1000}s`;
        }
    }

}
</script>

<template>
    <div class="timeline" :class="{'timeline--paused': isPaused}">
        <div v-for="(slide, index) in slides"
             :key="index"
             class="timeline__item"
             :style="{width: `calc(100% / ${slides.length})`}">
            <div class="timeline__itemBackground"></div>
            <div class="timeline__itemFill"
                 :class="{
                     'timeline__itemFill--past': index < currentIndex,
                     'timeline__itemFill--current': index === currentIndex
                 }"
                 :style="{'--duration': '2s'}"></div>
        </div>
    </div>
</template>

<style src="./story-timeline.css">

</style>