<template>
    <div>
    <h1>Loading...</h1>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    async setup() {
        const route = useRoute()
        const { data, error } = await useAsyncData(async () => {
            const GET_POST_BY_SLUG = `
        query GetPostBySlug($slug: String!) {
          postBy(slug: $slug) {
            id
            title
            excerpt
            featuredImage {
              node {
                sourceUrl
              }
            }
          }
        }
      `;
            const response = await axios.post(
                'https://vintagecarfanatics.com/graphql',
                {
                    query: GET_POST_BY_SLUG,
                    variables: { slug: route.params.slug },
                }
            );
            return response.data.data.postBy
        })
        let obj = Object.assign({}, data.value)
        useHead({
            title: obj.title,
            meta: [{
                property: "og:image",
                content:obj.featuredImage.node.sourceUrl
            }, {
                property: "og:title",
                content: obj.title
            }, {
                property: "og:description",
                content: obj.excerpt
            }, {
                property: "og:type",
                content: "website"
            }, {
                property: "og:url",
                content: "https://vintagecarfanatics.com/" + route.params.slug
            }]
        })
        const navigateToNewPage = () => {
            window.location.href = "https://vintagecarfanatics.com/" + route.params.slug
        }
        return {
            navigateToNewPage
        }


       //const nuxtApp = useNuxtApp()
       // await nuxtApp.callHook('route', "https://vintagecarfanatics.com/" + route.params.slug)
    },
        mounted() {
        this.navigateToNewPage()
    }
}
</script>
