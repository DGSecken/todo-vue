<script setup>

  import { reactive } from 'vue';

  import Cabecalho from './components/Cabecalho.vue'

  import Formulario from './components/Formulario.vue'

  import ListaDeTarefas from './components/ListaDeTarefas.vue'

const estado = reactive ({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
    titulo: 'estudar es6',
    finalizada: false,
  },
  {
    titulo: 'estudar sass',
    finalizada: false,
  },
  {
    titulo: 'academia',
    finalizada: true,
  }
]
})

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}

const tarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const tarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const tarefasFiltradas = () => {
  const {filtro} = estado;

  switch (filtro) {
    case 'pendentes':
      return tarefasPendentes();
    case 'finalizadas':
      return tarefasFinalizadas();
    default:
    return estado.tarefas;
  }
}

</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="tarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <ListaDeTarefas :tarefas="tarefasFiltradas()"/> 
  </div>
</template>

<style scoped>

.done {
  text-decoration: line-through;
}
</style>
