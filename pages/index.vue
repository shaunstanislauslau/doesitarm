<template>
    <section class="container py-24">
        <div class="flex flex-col items-center">
            <h1 class="title text-4xl md:text-6xl font-hairline leading-tight text-center">
                Does it ARM?
            </h1>
            <h2 class="subtitle md:text-xl font-light text-center">
                Apps that are reported to support Apple Silicon
            </h2>

            <LazyHydrate :on-interaction="['click', 'touchstart']">
                <Search />
            </LazyHydrate>

            <LazyHydrate :on-interaction="['click', 'touchstart']">
                <LinkButton
                    href="https://github.com/ThatGuySam/doesitarm/issues"
                >
                    Request an App
                </LinkButton>
            </LazyHydrate>
        </div>
    </section>
</template>

<script>
import LazyHydrate from 'vue-lazy-hydration'

import Search from '~/components/search.vue'
import LinkButton from '~/components/link-button.vue'

export default {
    components: {
        LazyHydrate,
        Search,
        LinkButton
    }
}
</script>
