<script>
export default {
    name: "StoriesViewer",

    props: ['stories'],
}
</script>

<template>
    <div class="stories__viewer">
        <div>close</div>
        <div class="stories__wrapper">
            <div class="stories__arrow">left arrow</div>
            <div class="stories__story">
                <div class="stories__timeline"></div>
                <div class="stories__storyWrap">
                    <img class="stories__storyImg" />
                </div>
            </div>
            <div class="stories__arrow">left arrow</div>
        </div>
    </div>
</template>

<style src="./stories-viewer.css" scoped>

</style>