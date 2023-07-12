<template>
    <div><h1>Hello</h1></div>
</template>

<script>
export default {
    data() {
        return {

        }
    },
    mounted() {
        this.$nextTick(() => {
            window.location.href = "https://vintagecarfanatics.com";
        })
    }
}
</script>