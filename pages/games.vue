<template>
    <section class="container py-24">
        <div class="flex flex-col items-center">
            <h1 class="title text-4xl md:text-6xl font-hairline leading-tight text-center">
                Does It ARM?
            </h1>
            <h2 class="subtitle md:text-xl text-center">
                Games that are reported to support Apple Silicon
            </h2>

            <ThomasCredit />

            <Search
                :app-list="gameList"
                :quick-buttons="quickButtons"
                no-email-subscribe
                @update:query="query = $event"
            />

            <LinkButton
                :href="`https://forms.gle/29GWt85i1G1L7Ttj8`"
            >
                Report a Game
            </LinkButton>
        </div>
    </section>
</template>

<script>
import Search from '~/components/search.vue'
import LinkButton from '~/components/link-button.vue'
import ThomasCredit from '~/components/thomas-credit.vue'

import gameList from '~/static/game-list.json'

export default {
    async asyncData () {
        const { sortedAppList, allList, allVideoAppsList, makeAppSearchLinks } = await import('~/helpers/get-list.js')
        const { default: gameList } = await import('~/static/game-list.json')
        const { default: videoList } = await import('~/static/video-list.json')

        return {
            // Map game list
            gameList: gameList.map( app => {

                return {
                    name: app.name,
                    status: app.status,
                    slug: app.slug,
                    // endpoint: app.endpoint,
                    text: app.text,
                    lastUpdated: app.lastUpdated,
                    category: app.category,
                    searchLinks: makeAppSearchLinks( app, (new Set(videoList)) )
                }
            })
        }
    },
    components: {
        Search,
        LinkButton,
        ThomasCredit
    },
    data: function () {
        return {
            query: '',
            quickButtons: [
                {
                    label: '✅ Native Support',
                    query: 'status:native'
                },
                {
                    label: '✳️ Rosetta',
                    query: 'status:rosetta'
                },
                {
                    label: '🚫 Unsupported',
                    query: 'status:no'
                }
            ]
        }
    },
    computed: {
        // gameList() {
        //     return gameList
        // }
    },
    head() {
        return {
            title: `Games supported on Apple Silicon - Does It ARM`,
            // meta: [
            //     // hid is used as unique identifier. Do not use `vmid` for it as it will not work
            //     {
            //         hid: 'description',
            //         name: 'description',
            //         content: 'My custom description'
            //     }
            // ]
        }
    }
}
</script>
