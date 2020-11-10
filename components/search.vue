<template>
    <div class="search w-full">
        <div class="search-input relative">
            <input
                id="search"
                ref="search"
                v-model="query"
                aria-label="Type here to Search"
                class="appearance-none w-full text-white font-hairline sm:text-5xl outline-none bg-transparent p-3"
                type="search"
                placeholder="Type to Search"
                autocomplete="off"
                @keyup="queryResults(query); scrollInputToTop()"
                @focus="hasFocused = true"
            >
            <div class="search-input-separator border-white border-t-2" />
            <div class="quick-buttons overflow-x-auto whitespace-no-wrap py-2 space-x-2">
                <button
                    v-for="button in quickButtons"
                    :key="button.query"
                    class="inline-block text-xs neumorphic-shadow-inner rounded-lg py-1 px-2"
                    @click="query = button.query; queryResults(query)"
                >{{ button.label }}</button>
            </div>
        </div>
        <div
            ref="search-container"
            class="search-container relative w-full"
        >
            <div class="search-wrapper flex justify-center">
                <div class="search-scroller w-full relative">

                    <div class="search-results py-8">

                        <ul class="results-container rounded-lg border border-gray-700 divide-y divide-gray-700 bg-gradient-to-br from-darker to-dark neumorphic-shadow-outer px-5">
                            <li
                                v-if="results.length === 0"
                                class="text-center py-4"
                            >
                                No apps found
                            </li>

                            <li
                                v-for="(app, i) in results"
                                :key="`${app.slug}-${i}`"
                            >
                                <SearchItem
                                    :app="app"
                                />
                            </li>

                        </ul>
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>

<script>
import scrollIntoView from 'scroll-into-view-if-needed'
import LazyHydrate from 'vue-lazy-hydration'

import appList from '~/assets/app-list.json'

import SearchItem from './search-item.vue'

// import overlayStore from './mixins/store'
// import modalRouter from '~/components/modals/mixins/router'
// import Card from '~/components/cards/Default.vue'
// import CardsRow from '~/components/cards/Row.vue'
// import ComingSoonImage from '~/components/partials/ComingSoonImage.vue'
// import InfoCircle from '~/assets/svg/info-circle.svg?inline'
// import PlayCircle from '~/assets/svg/play-circle.svg?inline'

export default {
    components: {
        LazyHydrate,
        SearchItem
    },
    props: {
        appList: {
            type: Array,
            default: () => appList
        },
        quickButtons: {
            type: Array,
            default: () => [
                {
                    label: 'Music Tools',
                    query: 'Music'
                },
                {
                    label: 'Developer Tools',
                    query: 'Developer'
                },
                {
                    label: 'Photo Tools',
                    query: 'Photo'
                },
                {
                    label: 'Video Tools',
                    query: 'Video'
                },
                {
                    label: 'Productivity Tools',
                    query: 'Productivity'
                },
            ]
        }
    },
    data: function () {
        return {
            // appList,
            query: '',
            // results: [],
            titleStartsWithResults: [],
            titleContainsResults: [],
            sectionContainsResults: [],

            hasFocused: false
        }
    },
    computed: {
        results () {
            if (!this.hasSearchInputText) return this.appList

            return [
                ...this.titleStartsWithResults,
                ...this.titleContainsResults,
                ...this.sectionContainsResults
            ]
        },
        hasSearchInputText () {
            return this.query.length > 0
        },
    },
    // watch: {
    //     'store.mode': function (newMode) {
    //         // If we're showing the search
    //         // then focus on the search input
    //         // on the next tick when our input
    //         // exists
    //         if (newMode === 'search') {
    //             this.$nextTick(() => {
    //                 this.$refs.search.focus()
    //             })
    //         }
    //     }
    // },
    // mounted () {
    // },
    methods: {
        // Search priorities
        titleStartsWith (query, app) {
            const matches = app.name.toLowerCase().startsWith(query)
            if (matches) {
                this.titleStartsWithResults.push(app)
            }
            return matches
        },
        titleContains (query, app) {
            const matches = app.name.toLowerCase().includes(query)
            if (matches) {
                this.titleContainsResults.push(app)
            }
            return matches
        },
        sectionContains (query, app) {
            const matches = app.section.label.toLowerCase().includes(query)
            if (matches) {
                this.sectionContainsResults.push(app)
            }
            return matches
        },
        // Search tools
        pluck (array, index) {
            const pluckedItem = array[index]
            array.splice(index, 1)
            return pluckedItem
        },
        scrollInputToTop () {
            scrollIntoView(this.$refs['search'], {
                block: 'start',
                behavior: 'smooth'
            })
        },
        queryResults (rawQuery) {
            // Clear any results from before
            this.titleStartsWithResults = []
            this.titleContainsResults = []
            this.sectionContainsResults = []


            // Snap results scroll position back to top
            this.$refs['search-container'].scrollTop = 0


            // If our query is empty
            // then bail
            if (rawQuery.length === 0) return
            const query = rawQuery.toLowerCase()


            // Search App List
            this.appList.forEach(app => {
                const matchers = [
                    this.titleStartsWith,
                    this.titleContains,
                    this.sectionContains
                ]

                // Run through our search priorities
                for (const method of matchers){
                    // iterations++
                    const appMatches = method(query, app)
                    if (appMatches) {
                        // We've found a match for this app
                        // so let's stop trying match methods
                        // and search the next app
                        break
                    }
                }
            })
            // console.log('query', query)
            // console.log('iterations', iterations)
        }
    }
}
</script>
