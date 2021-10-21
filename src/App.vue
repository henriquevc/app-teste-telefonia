<template>
    <div class="container mx-auto">
        <div class="mx-2 md:mx-auto md:max-w-lg lg:max-w-xl xl:max-w-2xl 2xl:max-w-2xl">
            <header class="text-5xl my-10 font-extrabold text-gray-800">
                <h1>App para testar a telefonia</h1>
            </header>
            <main>
                <section class="mb-6 border rounded-xl px-4 py-8">
                    <div class="flex items-center space-x-4 justify-between">
                        <ag-input v-model="host" type="url" :disabled="conectado" placeholder="Digite o host" class="w-full" id="host"/>
                        <ag-button v-if="!conectado" label="Conectar" @click="conectar" />
                        <div v-else class="text-lg text-white font-bold bg-green-800 py-3 px-3 rounded-full">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" viewBox="0 0 20 20" fill="currentColor">
                                <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" />
                            </svg>
                        </div>
                    </div>
                    <div class="ml-4 text-md font-semibold text-red-500" v-if="erroConexao">não foi possivel conectar</div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Campanhas</h2>
                    <div class="border rounded-xl px-4 sm:px-8 py-6 my-3 grid grid-cols-1 divide-y">
                        <div v-if="campanhas && campanhas.length">
                            <div v-for="(campanha, index) in campanhas" :key="index" class="text-lg py-4 px-2 font-semibold text-gray-800" :class="{'text-opacity-30': campanha.Ativa}">
                                {{campanha.Id}} - {{campanha.Nome}}
                            </div>
                        </div>
                        <lista-vazia v-else mensagem="esperando conectar host" />
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Login no ramal</h2>
                    <div class="border rounded-xl px-4 sm:px-8 py-6 my-3 flex justify-between items-center grid grid-cols-1 sm:grid-cols-2">
                        <ag-input
                            id="ramal"
                            type="number"
                            v-model="ramal"
                            class="w-full sm:w-3/4 mb-3 sm:mb-0"
                            placeholder="Digite o ramal"
                        />
                        <div class="flex space-x-4 justify-end">
                            <ag-button v-if="logado" label="Logout" @click="logout" />
                            <ag-button v-else label="Login" @click="login" />
                        </div>
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Motivos de pausa</h2>
                    <div class="border rounded-xl px-4 sm:px-8 py-6 my-3 grid grid-cols-1">
                        <div v-if="motivosPausa && motivosPausa.length" class="divide-y">
                            <div v-for="(motivo, index) in motivosPausa" :key="index" class="text-lg py-4 px-2 font-semibold flex items-center justify-between">
                                <span>{{motivo.Nome}}</span>
                                <ag-button v-if="!pausado" label="Pausar" @click="pausar(motivo.Id)" />
                            </div>
                        </div>
                        <lista-vazia v-else mensagem="lista de motivos de pausa vazia" />
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Discar</h2>
                    <div
                        class="border rounded-xl px-4 sm:px-8 py-6 my-3 space-y-4"
                    >
                        <ag-input
                            type="tel"
                            v-model="telefone"
                            class="w-full"
                            :v-mask="['(##) ####-####', '(##) #####-####']"
                            placeholder="(XX) XXXXX-XXXX"
                        />
                        <div class="w-full flex space-x-1.5 sm:space-x-8 justify-end">
                            <ag-button color="red" label="Desligar" />
                            <ag-button color="green" label="Ligar" />
                        </div>
                    </div>
                </section>
            </main>
        </div>
    </div>
</template>

<script>
import AgButton from './components/AgButton.vue'
import AgInput from './components/AgInput.vue'
import ListaVazia from './components/ListaVazia.vue'
import axios from 'axios'
import { mask } from 'vue-the-mask'

export default {
    components: { AgButton, AgInput, ListaVazia },
    directives: { mask },
    name: 'App',
    data() {
        return {
            ramal: '',
            telefone: '',
            host: '',
            conectado: false,
            erroConexao: false,
            logado: false,
            erroLogin: false,
            pausado: false,
            erroPausa: false,
            motivosPausa: [],
            campanhas: []
        }
    },
    methods: {
        conectar () {
            if (this.host.endsWith('/')) {
                this.host = this.host.slice(0, -1)
            }
            this.erroConexao = false
            axios.get(this.host)
                .then(response => {
                    this.conectado = true
                    this.erroConexao = false
                    this.listarCampanhas()
                }).catch(error => {
                    console.error(error)
                    this.conectado = false
                    this.erroConexao = true
                })
        },
        listarCampanhas () {
            axios.get(`${this.host}/ListarCampanhas`)
                .then(response => {
                    this.campanhas = response.data
                })
                .catch(error => {
                    this.campanhas = []
                    console.log(error)
                })
        },
        login () {
            axios.request({
                url: `${this.host}/Login`,
                params: {
                    ramal: this.ramal
                },
                method: 'GET'
            }).then(() => {
                this.logado = true
                this.listarMotivosPausa()
            }).catch(error => {
                this.logado = false
                // this.motivosPausa = [
                //     {Id: 0, Nome: 'Banheiro'},
                //     {Id: 1, Nome: 'Almoço'},
                //     {Id: 2, Nome: 'Reunião'},
                //     {Id: 3, Nome: 'Lanche'},
                //     {Id: 4, Nome: 'F1'}
                // ]
                console.log(error)
            })
        },
        logout () {
            axios.request({
                url: `${this.host}/Logout`,
                params: {
                    ramal: this.ramal
                },
                method: 'GET'
            }).then(() => {
                this.logado = false
                this.motivosPausa = []
            }).catch(error => {
                this.logado = true
                console.log(error)
            })
        },
        listarMotivosPausa () {
            axios.request({
                url: `${this.host}/ListarMotivosPausa`,
                params: {
                    ramal: this.ramal
                },
                method: 'GET'
            }).then(response => {
                this.motivosPausa = response.data
            })
            .catch(error => {
                this.motivosPausa = []
                console.log(error)
            })
        },
        pausar (pausaId) {
            console.log(pausaId)
        },
        despausar () {
            console.log('despausar não implementada')
        }
    }
}
</script>
