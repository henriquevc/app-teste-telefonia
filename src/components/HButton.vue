<template>
    <button
        :class="`bg-${color}-800 hover:bg-${color}-600 border-none focus:border-indigo-700 outline-none text-lg text-white py-3 px-6 font-bold rounded-xl  focus:ring focus:ring-gray-500`"
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
