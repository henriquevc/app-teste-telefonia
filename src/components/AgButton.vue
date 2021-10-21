<template>
    <button
        :class="`bg-${color}-800 hover:bg-${color}-600 border-none focus:border-indigo-700 outline-none text-lg text-white py-2 px-4 sm:py-3 sm:px-6 font-bold rounded-lg focus:ring focus:ring-${color}-500 focus:ring-offset-1 disabled:opacity-70`"
    >
        {{ label }}
    </button>
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        label: String,
        color: {
            String,
            default: 'gray'
        }
    },
    data() {
        return {
            count: 0
        }
    }
}
</script>
