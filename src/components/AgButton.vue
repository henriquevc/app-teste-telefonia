<template>
    <button
        :class="`${flat ? 'bg-white text-' + color + '-800' : 'bg-' + color + '-800 text-white'} ${flat ? '' : 'hover:bg-' + color + '-600'} focus:border-${color}-700 text-lg py-2 px-4 sm:py-3 sm:px-6 font-bold rounded-lg focus:ring focus:ring-${color}-500 focus:ring-offset-1 disabled:opacity-70 inline-flex items-center`"
    >
        <span v-if="remove">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
            </svg>
        </span>
        <span v-else>{{ label }}</span>
    </button>
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        label: {
            type: String,
            required: false
        },
        color: {
            type: String,
            default: 'gray'
        },
        remove: {
            type: Boolean,
            default: false,
            required: false
        },
        flat: {
            type: Boolean,
            default: false,
            required: false
        }
    },
    data() {
        return {
            count: 0
        }
    }
}
</script>
