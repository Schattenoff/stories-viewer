<script>
import StoriesViewer from "@/components/stories/viewer/main/StoriesViewer.vue";

export default {
    name: "Stories",

    components: {
        StoriesViewer,
    },

    data() {
        return {
            isShowViewer: false,
            activeStoryId: null,
            stories: [
                {
                    id: 1,
                    previewImg: 'https://i.postimg.cc/qqd0MM3r/1-2.png',
                    images: [
                        {
                            id: 1,
                            storyImg: '',
                            caption: '',
                            action: {
                                background: '',
                                color: '',
                                text: 'Применить промокод'
                            },
                        }
                    ]
                },
                {
                    id: 2,
                    previewImg: 'https://i.postimg.cc/ncpZyrxY/2.png',
                    images: [
                        {
                            id: 1,
                            storyImg: '',
                            caption: '',
                            action: {
                                background: '',
                                color: '',
                                text: 'Применить промокод'
                            },
                        }
                    ]
                },
                {
                    id: 3,
                    previewImg: 'https://i.postimg.cc/7ZL4LRf5/3.png',
                    images: [
                        {
                            id: 1,
                            storyImg: '',
                            caption: '',
                            action: {
                                background: '',
                                color: '',
                                text: 'Применить промокод'
                            },
                        }
                    ]
                },
                {
                    id: 4,
                    previewImg: 'https://i.postimg.cc/dt6vwFtM/4.png',
                    images: [
                        {
                            id: 1,
                            storyImg: '',
                            caption: '',
                            action: {
                                background: '',
                                color: '',
                                text: 'Применить промокод'
                            },
                        }
                    ]
                },
            ]
        }
    },

    methods: {
        handleClickStory(id) {
            this.activeStoryId = id;
            this.isShowViewer = true;
        }
    }
}
</script>

<template>

    <div class="stories">

        <div v-for="story in stories"
             :key="story.id"
             class="stories__card" @click="handleClickStory(story.id)">
            <div class="stories__cardWrap">
                <img class="stories__cardImg" :src="story.previewImg" />
            </div>
        </div>

        <StoriesViewer v-show="isShowViewer" :stories="stories" />

    </div>



</template>

<style src="./stories.css" scoped>
</style>