<template>
    <div ref="wrapperEl" class="wrapper" :class="[index % 2 !== 0 ? 'odd': 'even']">
        <div ref="typeEl" class="text">Type: {{ data.type }}</div>
        <div ref="meaningEl" class="text">Meaning: {{ data.meaning }}</div>
    </div>
</template>

<script setup>
    import { defineProps, onMounted, ref } from 'vue'
    import { gsap } from 'gsap'

    const props = defineProps({
        data: {
            type: Object,
            required: true
        },
        index: {
            type: Number,
            required: true
        }
    })

    const wrapperEl = ref(null)
    const typeEl = ref(null)
    const meaningEl = ref(null)

    onMounted(() => {
        // Initial state for wrapper
        gsap.set(wrapperEl.value, { autoAlpha: 0 });

        // Initial state for texts: hidden and shifted to the left
        gsap.set([typeEl.value, meaningEl.value], { autoAlpha: 0, x: -10 });

        // Animate wrapper fade in
        gsap.to(wrapperEl.value, {
            duration: 0.5,
            autoAlpha: 1,
            onComplete: () => {
                // Animate texts coming in from the left and fading in, one after the other
                gsap.to(typeEl.value, { duration: 0.5, autoAlpha: 1, x: 0, delay: 0.2 });
                gsap.to(meaningEl.value, { duration: 0.5, autoAlpha: 1, x: 0, delay: 0.7 });
            }
        })
    })
</script>

<style scoped>
    .wrapper {
        display: flex;
        flex-direction: column;
        gap: .2rem;
        padding: 6px 10px;
        border-radius: 6px;
        background-color: aliceblue;
        font-size: 14px;
        min-width: 10rem;
        position: absolute;
    }

    .even {
        top: 50%;
        left: 14px;
        /* background-color: green !important; */
    }

    .odd {
        bottom: -40px !important;
        left: 14px;
        /* background-color: red !important; */
    }
</style>