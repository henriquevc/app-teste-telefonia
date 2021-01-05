<template>
    <div class="container mx-auto">
        <div
            class="mx-auto xs:w-32 sm:px-8 lg:max-w-xl xl:max-w-2xl 2xl:max-w-2xl"
        >
            <header class="text-5xl my-10 font-extrabold text-gray-800">
                <h1>App para testar a telefonia</h1>
            </header>
            <main>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Login</h2>
                    <div
                        class="border rounded-xl px-8 py-6 my-3 flex justify-between"
                    >
                        <input
                            type="text"
                            v-model="ramal"
                            class="max-w-3xl pl-4 font-semibold"
                            placeholder="Digite o ramal"
                        />
                        <h-button label="Entrar" />
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Logout</h2>
                    <div
                        class="border rounded-xl px-8 py-6 my-3 flex justify-between"
                    >
                        <input
                            type="text"
                            v-model="ramal"
                            class="max-w-3xl pl-4 font-semibold"
                            placeholder="Digite o ramal"
                        />
                        <h-button label="Sair" />
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Discar</h2>
                    <div
                        class="border rounded-xl px-8 py-6 my-3 flex justify-between"
                    >
                        <input
                            type="tel"
                            v-model="telefone"
                            class="max-w-3xl pl-4 font-semibold"
                            placeholder="Digite o telefone"
                        />
                        <h-button label="Ligar"></h-button>
                    </div>
                </section>
            </main>
        </div>
    </div>
</template>

<script>
import HButton from './components/HButton.vue'

export default {
    components: { HButton },
    name: 'App',
    data() {
        return {
            ramal: '',
            telefone: ''
        }
    }
}
</script>
