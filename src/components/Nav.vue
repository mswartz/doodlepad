<template>

</template>

<script>
export default {
    name: 'Nav',
    methods: {
        login () {
            this.$emit('login')
        },
        logout () {
            this.$emit('logout')
        }
        },
        data () {
        return {
        content: ''
        }
    }
}
</script>
