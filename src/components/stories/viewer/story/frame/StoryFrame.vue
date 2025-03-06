<script>
export default {
    name: "StoryFrame",

    props: ['frameUrl'],
}
</script>

<template>
    <div class="frame">
        <img class="frame__img" :src="frameUrl" />
    </div>
</template>

<style src="./story-frame.css" scoped>

</style>