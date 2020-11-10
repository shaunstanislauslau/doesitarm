<template>
    <a
        :href="`/app/${ app.slug }`"
        class="block hover:neumorphic-shadow hover:bg-gradient-to-br from-darkest to-dark focus:outline-none focus:bg-gray-50 transition duration-300 ease-in-out rounded-lg -mx-5"
    >
        <div class="flex items-center px-4 py-4 sm:px-6">
            <div class="min-w-0 flex-1 flex items-center">
                <div class="flex-shrink-0">
                    <div
                        class="h-12 w-12 rounded-full flex items-center justify-center bg-darker"
                    >
                        <div>{{ app.name.charAt(0) }}</div>
                    </div>
                </div>
                <div class="min-w-0 flex-1 px-4 md:grid md:grid-cols-2 md:gap-4">
                    <div>
                        <div class="text-sm leading-5 font-light truncate">{{ app.name }}</div>
                        <div class="mt-2 flex items-center text-sm leading-5 text-gray-500">
                            <span class="">{{ app.text }}</span>
                        </div>
                    </div>
                    <div class="hidden md:block">
                        <div>
                            <!-- <div class="text-sm leading-5 text-gray-900">
                                Applied on
                                <time datetime="2020-01-07">January 7, 2020</time>
                            </div> -->
                            <!-- <div class="mt-2 flex items-center text-sm leading-5 text-gray-500">
                                {{ app.text }}
                            </div> -->
                        </div>
                    </div>
                </div>
            </div>
            <div>
                <!-- Heroicon name: chevron-right -->
                <svg
                    class="h-5 w-5 text-gray-400"
                    viewBox="0 0 20 20"
                    fill="currentColor">
                    <path
                        fill-rule="evenodd"
                        d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                        clip-rule="evenodd" />
                </svg>
            </div>
        </div>
    </a>
</template>

<script>
export default {
    props: {
        app: {
            type: Object,
            required: true
        },
    }
}
</script>
