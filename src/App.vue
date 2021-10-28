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
                        <div v-else class="text-lg text-white flex items-center font-bold bg-green-800 py-3 px-3 rounded-full">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-1" viewBox="0 0 20 20" fill="currentColor">
                                <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" />
                            </svg>
                            conectado
                        </div>
                    </div>
                    <div class="ml-4 text-md font-semibold text-red-500" v-if="erroConexao">não foi possivel conectar</div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Campanhas</h2>
                    <div class="border rounded-xl px-4 sm:px-8 py-6 my-3 grid grid-cols-1">
                        <div v-if="campanhas && campanhas.length" class="divide-y">
                            <div v-for="(campanha, index) in campanhas" :key="index" class="text-lg py-4 px-2 font-semibold text-gray-800" :class="{'text-opacity-30': !campanha.Ativa}">
                                <div>{{campanha.Id}} - {{campanha.Nome}}</div>
                            </div>
                        </div>
                        <lista-vazia v-else mensagem="esperando conectar host" />
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Listar tabulações</h2>
                    <div class="border rounded-xl px-4 sm:px-8 py-6 my-3 grid grid-cols-1">
                        <div>
                            <div class="flex items-center justify-between">
                                <ag-input v-model="campanhaId" id="campanhaId" type="tel" placeholder="Digite o ID da campanha" />
                                <ag-button label="Buscar" @click="listarTabulacoes" l />
                            </div>
                            <div v-if="tabulacoes && tabulacoes.length" class="divide-y mt-4">
                                <div v-for="(tabulacao, index) in tabulacoes" :key="index" class="text-lg py-4 px-2 font-semibold flex items-center justify-between">
                                    <div>{{tabulacao.Id}} - {{tabulacao.Nome}}</div>
                                </div>
                            </div>
                            <lista-vazia v-else mensagem="lista de tabulações vazia" />
                        </div>
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Login no ramal</h2>
                    <div class="border rounded-xl px-4 sm:px-8 py-6 my-3 flex justify-between items-center grid grid-cols-1 sm:grid-cols-2">
                        <ag-input
                            id="ramal"
                            type="number"
                            v-model="ramal"
                            :disabled="logado"
                            class="w-full mb-3 sm:mb-0"
                            placeholder="Digite o ramal"
                        />
                        <div class="flex space-x-4 justify-end">
                            <ag-button v-if="logado" label="Logout" @click="logout" />
                            <ag-button v-else label="Login" @click="login" />
                        </div>
                        <span v-if="logado" class="text-green-600 font-bold ml-3 mt-1">Logado</span>
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Motivos de pausa</h2>
                    <div class="border rounded-xl px-4 sm:px-8 py-6 my-3 grid grid-cols-1">
                        <div v-if="motivosPausa && motivosPausa.length" class="divide-y">
                            <div v-if="pausado" class="flex items-center justify-between">
                                <div>
                                    <span class="text-sm opacity-70">Ramal pausado em:</span>
                                    <p class="text-2xl font-semibold">{{motivoPausado.Nome}}</p>
                                </div>
                                <ag-button label="Tirar da pausa" @click="despausar"/>
                            </div>
                            <div v-if="!pausado">
                                <div v-for="(motivo, index) in motivosPausa" :key="index" class="text-lg py-4 px-2 font-semibold flex items-center justify-between">
                                    <span>{{motivo.Id}} - {{motivo.Nome}}</span>
                                    <ag-button label="Pausar" @click="pausar(motivo.Id)" />
                                </div>
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
                            v-maska="['+55 (##) ####-####', '+55 (##) #####-####']"
                            @maska="telefoneRaw = $event.target.dataset.maskRawValue"
                            placeholder="(XX) XXXXX-XXXX"
                        />

                        <div class="w-full flex space-x-1.5 sm:space-x-8 justify-end">
                            <div v-if="discando" class="text-lg font-bold">Discando...</div>
                            <div v-if="!discando">
                                <ag-button v-if="emLigacao" color="red" label="Desligar" @click="desligar"/>
                                <ag-button v-if="!emLigacao" color="green" label="Discar" @click="discar" />
                            </div>
                        </div>
                    </div>
                </section>
                <section class="mb-6">
                    <h2 class="text-3xl font-medium">Relatório de chamadas</h2>
                    <div
                        class="border rounded-xl px-4 sm:px-8 py-6 my-3 divide-y"
                    >
                        <div>
                            <span>Telefones de filtro para o relatório:</span>
                            <div v-for="(item, index) in telefones" :key="index" class="flex space-x-2 mt-2">
                                <ag-input
                                    type="tel"
                                    v-model="item.telefone"
                                    class="w-full"
                                    v-maska="['+55 (##) ####-####', '+55 (##) #####-####']"
                                    @maska="item.telefoneRaw = $event.target.dataset.maskRawValue"
                                    placeholder="(XX) XXXXX-XXXX"
                                />
                                <ag-button remove flat color="red" @click="removeTelefone(index)" />
                            </div>
                            <div class="flex justify-end my-4">
                                <ag-button label="adicionar telefone" @click="addTelefone" />
                            </div>
                        </div>
                        <div class="flex space-x-1.5 sm:space-x-8 justify-end pt-8" >
                            <ag-button label="relatório simplificado" @click="extrairRelatorioChamadasSimplificado"/>
                            <ag-button label="relatório completo" @click="extrairRelatorioChamadasCompleto"/>
                        </div>
                        <div class="fixed top-0 left-0 w-screen h-screen flex items-center justify-center" v-if="modalRelatorioCompleto">
                            <div class="bg-white w-full h-full p-12 overflow-auto"> 
                                <div class="text-2xl mb-8 flex justify-between items-center">
                                    Relatório de chamadas
                                    <ag-button label="X" @click="modalRelatorioCompleto = false" flat />
                                </div>
                                <table class="table-auto border border-collapse w-full">
                                    <thead>
                                        <tr>
                                            <th class="border px-2">Id</th>
                                            <th class="border px-2">Identificador Gravação</th>
                                            <th class="border px-2">Tipo</th>
                                            <th class="border px-2">Status</th>
                                            <th class="border px-2">Data</th>
                                            <th class="border px-2">Origem</th>
                                            <th class="border px-2">Destino</th>
                                            <th class="border px-2">Duração Atendimento</th>
                                            <th class="border px-2">Duração Ligação</th>
                                            <th class="border px-2">Usuário</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(item, index) in relatorioChamadas" :key="index">
                                            <td class="border px-2">{{item.Id}}</td>
                                            <td class="border px-2">{{item.IdentificadorGravacao}}</td>
                                            <td class="border px-2">{{item.Tipo}}</td>
                                            <td class="border px-2">{{item.Status}}</td>
                                            <td class="border px-2">{{item.Data}}</td>
                                            <td class="border px-2">{{item.Origem}}</td>
                                            <td class="border px-2">{{item.Destino}}</td>
                                            <td class="border px-2">{{item.DuracaoAtendimento}}</td>
                                            <td class="border px-2">{{item.DuracaoLigacao}}</td>
                                            <td class="border px-2">{{item.Usuario}}</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="mt-8 px-4 divide-y">

                            <div v-for="(item, index) in relatorioChamadas" :key="index" class="pt-4">
                                <div class="w-full flex flex-wrap mb-1 items-start">
                                    <div class="w-full text-sm text-gray-800 font-semibold">{{item.Data}}</div>
                                    <div class="w-4/12 mt-1">
                                        <div class="text-sm text-gray-400">Origem</div>
                                        <div class="text-gray-800 text-lg font-semibold">{{item.Origem}}</div>
                                    </div>
                                    <div class="w-4/12 mt-1">
                                        <div class="text-sm text-gray-400">Destino</div>
                                        <div class="text-gray-800 text-lg font-semibold">{{item.Destino}}</div>
                                    </div>
                                    <div class="w-4/12 mt-1">
                                        <div class="text-sm text-gray-400">Duração Atendimento</div>
                                        <div class="text-gray-800 text-lg font-semibold">{{item.DuracaoAtendimento}}</div>
                                    </div>
                                    <div class="w-4/12 mt-1">
                                        <div class="text-sm text-gray-400">Usuário</div>
                                        <div class="text-gray-800 text-lg font-semibold">{{item.Usuario}}</div>
                                    </div>        
                                    <div class="w-4/12 mt-1">
                                        <div class="text-sm text-gray-400">Status</div>
                                        <div class="text-gray-800 text-lg font-semibold">{{item.Status}}</div>
                                    </div>
                                    <div class="w-4/12 mt-1">
                                        <div class="text-sm text-gray-400">Duração Ligação</div>
                                        <div class="text-gray-800 text-lg font-semibold">{{item.DuracaoLigacao}}</div>
                                    </div>
                                </div>
                            </div>
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
import { maska } from 'maska'

export default {
    components: { AgButton, AgInput, ListaVazia },
    directives: { maska },
    name: 'App',
    data() {
        return {
            host: import.meta.env.VITE_HOST,
            ramal: import.meta.env.VITE_RAMAL,
            campanhaId: '',
            telefone: '',
            telefoneRaw: '',
            telefones: [
                {telefone: '', telefoneRaw: ''}
            ],
            conectado: false,
            erroConexao: false,
            logado: false,
            erroLogin: false,
            pausado: false,
            erroPausa: false,
            emLigacao: false,
            discando: false,
            motivosPausa: [],
            motivoPausado: {},
            campanhas: [],
            tabulacoes: [],
            relatorioChamadas: [],
            modalRelatorioCompleto: false
        }
    },
    mounted () {
        if (this.host) {
            this.conectar()
        }
        if (this.ramal) {
            this.login()
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
                    console.error(error)
                })
        },
        login () {
            axios.request({
                url: `${this.host}/Login`,
                params: {
                    ramal: this.ramal
                },
                method: 'POST'
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
                console.error(error)
            })
        },
        logout () {
            axios.request({
                url: `${this.host}/Logout`,
                params: {
                    ramal: this.ramal
                },
                method: 'POST'
            }).then(() => {
                this.logado = false
                this.motivosPausa = []
            }).catch(error => {
                this.logado = true
                console.error(error)
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
                console.error(error)
            })
        },
        listarTabulacoes () {
            this.tabulacoes = []
            axios.request({
                url: `${this.host}/ListarTiposTabulacao`,
                params: {
                    campanha: this.campanhaId
                },
                method: 'GET'
            }).then(response => {
                this.tabulacoes = response.data
            }).catch(error => {
                console.error(error)
            })
        },
        pausar (pausaId, excecaoRetornoPausa = false) {
            axios.request({
                url: `${this.host}/Pausar`,
                params: {
                    ramal: this.ramal,
                    idMotivoPausa: pausaId
                },
                method: 'POST'
            }).then(response => {
                if (!excecaoRetornoPausa) {
                    this.pausado = true
                }
                this.motivoPausado = this.motivosPausa.find(mp => mp.Id === pausaId)
            })
            .catch(error => {
                this.pausado = false
                console.error(error)
            })
        },
        despausar () {
             axios.request({
                url: `${this.host}/RetornarPausa`,
                params: {
                    ramal: this.ramal
                },
                method: 'POST'
            }).then(() => {
                this.pausado = false
                this.pausar(109, true)
            })
            .catch(error => {
                this.pausado = true
                console.error(error)
            })
        },
        discar () {
            this.discando = true
            axios.request({
                url: `${this.host}/Discar`,
                params: {
                    ramal: this.ramal,
                    numero: this.telefoneRaw,
                    idCrm: 42
                },
                method: 'POST'
            }).then(() => {
                this.emLigacao = true
            }).catch(error => {
                console.error(error)
                this.emLigacao = false
            }).finally(() => {
                this.discando = false
            })
        },
        desligar () {
             axios.request({
                url: `${this.host}/Desligar`,
                params: {
                    ramal: this.ramal
                },
                method: 'POST'
            }).then(() => {
                this.emLigacao = false
            }).catch(error => {
                console.error(error)
                this.emLigacao = true
            })
            this.emLigacao = false
        },
        addTelefone () {
            this.telefones.push({telefone: '', telefoneRaw: ''})
        },
        removeTelefone (index) {
            this.telefones.splice(index, 1)
        },
        extrairRelatorioChamadasSimplificado () {
            this.buscarRelatorio()
                .then(response => {
                    this.relatorioChamadas = response.data
                    this.modalRelatorioCompleto = false
                }).catch(error => {
                    console.error(error)
                })
        },
        extrairRelatorioChamadasCompleto () {
            this.buscarRelatorio()
                .then(response => {
                    this.relatorioChamadas = response.data
                    this.modalRelatorioCompleto = true
                }).catch(error => {
                    console.error(error)
                })
        },
        async buscarRelatorio () {
            let numeros = this.telefones.map(tel => tel.telefoneRaw).join(';')
            return new Promise((resolve, reject) => {
                axios.request({
                    url: `${this.host}/Relatorio/Chamadas`,
                    params: {
                        numeros: numeros
                    },
                    method: 'GET'
                }).then(response => {
                    resolve(response)
                }).catch(error => {
                    reject(error)
                })
            })
        }
    }
}
</script>
