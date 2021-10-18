<template>
    <div class="container mx-auto">
        <div
            class="mx-2 md:mx-auto md:max-w-lg lg:max-w-xl xl:max-w-2xl 2xl:max-w-2xl"
        >
            <header class="text-5xl my-10 font-extrabold text-gray-800">
                <h1>App para testar a telefonia</h1>
            </header>
            <main>
                <section class="mb-6 flex items-center space-x-4 justify-between border rounded-xl px-4 py-8">
                    <div class="space-y-4">
                        <input type="text" class="font-semibold border pl-4 py-3 w-full" placeholder="Digite o host">
                        <input type="text" class="font-semibold border pl-4 py-3 w-full" placeholder="Digite o token">
                    </div>
                    <div>
                        <h-button label="Validar" />
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Login</h2>
                    <div
                        class="border rounded-xl px-4 sm:px-8 py-6 my-3 flex justify-between"
                    >
                        <input
                            type="text"
                            v-model="ramal"
                            class="w-1/2 max-w-3xl pl-2 sm:pl-4 font-semibold border"
                            placeholder="Digite o ramal"
                        />
                        <h-button clas="w-1/4" label="Login" />
                        <h-button clas="w-1/4" label="Logout" />
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Discador</h2>
                    <div
                        class="border rounded-xl px-4 sm:px-8 py-6 my-3 space-y-4"
                    >
                        <div class="flex w-full">
                            <input
                                type="tel"
                                v-model="telefone"
                                class="w-full pl-4 py-3 font-semibold border"
                                placeholder="Digite o telefone"
                            />
                        </div>
                        <div class="w-full flex space-x-2 justify-between">
                            <h-button color="yellow" label="Desligar"></h-button>
                            <h-button color="green" label="Ligar"></h-button>
                            <h-button label="Pausar"></h-button>
                        </div>
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Motivos de pausa</h2>
                    <div
                        class="border rounded-xl px-4 sm:px-8 py-6 my-3"
                    >
                    <div v-for="(motivo, index) in motivosPausa" :key="index">
                        {{motivo.nome}}
                    </div>
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
            telefone: '',
            host: '',
            token: '',
            motivosPausa: [
                {nome: 'Banheiro'}, 
                {nome: 'Descanso'}, 
                {nome: 'Café'}]
        }
    }
}
</script>
