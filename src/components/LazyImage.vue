<template>
    <div
        class="w-full relative min-h-24"
        :class="`lazy-image-${id}`"
    >
        <img
            v-if="loadedImage"
            :src="loadedImage"
            :class="imageClasses"
            class="relative z-10"
        />

        <slot v-else />
    </div>
</template>

<script>
    import { useId } from 'vue'

    export default {
        props: {
            image: {
                required: true,
            },
            imageClasses: {
                default: '',
            },
        },

        computed: {
            id()
            {
                return useId()
            }
        },

        data() {
            return {
                loadedImage: null,
            }
        },

        mounted()
        {
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        this.loadedImage = this.image
                        observer.unobserve(entry.target)
                    }
                })
            })

            observer.observe(document.querySelector(`.lazy-image-${this.id}`))
        }
    }
</script>