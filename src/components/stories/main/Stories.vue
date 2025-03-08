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
                            src: 'https://images.unsplash.com/photo-1482049016688-2d3e1b311543?q=80&w=2020&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                            actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'Применить промокод'
                                }
                            ],
                        },
                        {
                            id: 2,
                            src: 'https://images.unsplash.com/photo-1467003909585-2f8a72700288?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                              actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'В корзину'
                                }
                            ],
                        },
                    ]
                },
                {
                    id: 2,
                    previewImg: 'https://i.postimg.cc/ncpZyrxY/2.png',
                    images: [
                        {
                            id: 3,
                            src: 'https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?q=80&w=1981&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                              actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'Применить промокод'
                                }
                            ],
                        },
                        {
                            id: 4,
                            src: 'https://images.unsplash.com/photo-1565958011703-44f9829ba187?q=80&w=1965&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                              actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'Применить промокод'
                                }
                            ],
                        }
                    ]
                },
                {
                    id: 3,
                    previewImg: 'https://i.postimg.cc/7ZL4LRf5/3.png',
                    images: [
                        {
                            id: 2,
                            src: 'https://images.unsplash.com/photo-1467003909585-2f8a72700288?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                            actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'Применить промокод'
                                }
                            ],
                        },
                    ]
                },
                {
                    id: 4,
                    previewImg: 'https://i.postimg.cc/dt6vwFtM/4.png',
                    images: [
                        {
                            id: 1,
                            src: 'https://images.unsplash.com/photo-1482049016688-2d3e1b311543?q=80&w=2020&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                              actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'Применить промокод'
                                }
                            ],
                        },
                        {
                            id: 2,
                            src: 'https://images.unsplash.com/photo-1467003909585-2f8a72700288?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                              actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'Применить промокод'
                                }
                            ],
                        },
                        {
                            id: 3,
                            src: 'https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?q=80&w=1981&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                              actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'Применить промокод'
                                }
                            ],
                        },
                        {
                            id: 4,
                            src: 'https://images.unsplash.com/photo-1565958011703-44f9829ba187?q=80&w=1965&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
                            caption: '',
                              actions: [
                                {
                                    placement: {
                                        x: '',
                                        y: ''
                                    },
                                    background: '',
                                    color: '',
                                    text: 'Применить промокод'
                                }
                            ],
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
        },

        onClose() {
            this.isShowViewer = false;
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

        <StoriesViewer v-if="isShowViewer"
                       :active="activeStoryId"
                       :stories="stories"
                       @close="onClose" />

    </div>



</template>

<style src="./stories.css" scoped>
</style>