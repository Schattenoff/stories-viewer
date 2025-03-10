<script>
export default {
    name: "StoryFrame",

    props: ['frameSrc'],
}
</script>

<template>
    <div class="frame">
        <img class="frame__img" :src="frameSrc" />
    </div>
</template>

<style src="./story-frame.css" scoped>

</style>