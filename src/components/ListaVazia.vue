<template>
    <div class="flex justify-center my-4">
        <span class="text-lg font-semibold text-red-500 border-2 px-4 py-0.5 rounded-2xl border-red-500">{{mensagem}}</span>
    </div>
</template>
<script>
export default {
    props: {
        mensagem: {
            type: String,
            required: true
        }
    }
}
</script>
