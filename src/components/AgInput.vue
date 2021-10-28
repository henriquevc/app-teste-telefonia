<template>
    <input
        :id="id"
        :type="type"
        label="Host"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        class="font-semibold border rounded-md pl-4 py-3 focus:ring-grey-500"
    />
</template>

<script>
export default {
    props: {
        type: {
            type: String,
            required: true,
            default: 'text'
        },
        modelValue: {
            required: true
        },
        id: {
            type: String,
            required: false
        }
    }
}
</script>