<script setup>
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  BarElement,
  CategoryScale,
  LinearScale
} from 'chart.js'
import { Bar } from 'vue-chartjs'
import { useStore } from '../stores/mainStore';
import { computed } from 'vue';

const props = defineProps({
    labels: Array,
    dataSet: Array,
})
let store = useStore()

ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip)
    let data = {
        labels: props.labels,
        datasets: [
            {
                data: props.dataSet,
                
            }
            
        ],
    }
    
    let options = {
        indexAxis: 'y',
        responsive: true,
        backgroundColor: [
        'rgba(255, 99, 132, 0.2)',
        'rgba(255, 159, 64, 0.2)',
        'rgba(255, 205, 86, 0.2)',
        'rgba(75, 192, 192, 0.2)',
        'rgba(54, 162, 235, 0.2)',
        ],
        borderColor: [
        'rgb(255, 99, 132)',
        'rgb(255, 159, 64)',
        'rgb(255, 205, 86)',
        'rgb(75, 192, 192)',
        'rgb(54, 162, 235)',
        ],
        borderWidth: 1,
        layout: {
            padding: 20
        },
    }
 

    // Function close modal
    const closeModal = (e) => {
        if(e.target.className.includes('modalWrapper')){
            store.modalShow = false
        }
    }


</script>
<template>
    <div class="modalWrapper" @click="(e) => closeModal(e)">
        <div class="modalWindow">
            <div class="chart">
                <Bar :data="data" :options="options"/>
            </div>
        </div>
    </div>
</template>
<style lang="less">
    .modalWrapper{
        overflow-y: hidden;
        position: fixed;
        z-index: 100;
        width: 100%;
        height: 100vh;
        top: 0;
        left: 0;
        background-color: rgba(0, 0, 0, 0.692);
        display: flex;
        align-items: center;
        justify-content: center;

        .modalWindow{
            width: 1000px;
            max-width: 94%;
            background-color: #242424;
            box-shadow: 1px 1px 10px 5px #151e10ed;
            border-radius: 10px;

        }
    }
  
</style>