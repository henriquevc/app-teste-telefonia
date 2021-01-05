<template>
    <button
        class="bg-gray-800 hover:bg-gray-600 border-none focus:border-indigo-700 outline-none text-lg text-white py-3 px-6 font-bold rounded-xl"
    >
        {{ label }}
    </button>
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        label: String
    },
    data() {
        return {
            count: 0
        }
    }
}
</script>
