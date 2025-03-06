<script>
export default {
    name: "StoryAction",

    props: ['action'],
}
</script>

<template>
    <div class="action">
        {{ action.text }}
    </div>
</template>

<style src="./story-action.css" scoped>
</style>