<script setup>
import { ref, defineProps, computed, watch } from 'vue';
import { useStore } from '../stores/mainStore';
import ChartModal from './ChartModal.vue'




const props = defineProps({
    posts: Array,
})
const store = useStore()
let pageNumber = ref(0);
let size = 10;
let search = ref('');
let postsArray = ref(props.posts)
let modalActive = computed(() => {
    return store.modalShow
})
let chartDataSet = ref()
let chartLabels = ref()

// function for switch page
const switchPage = (count) => {
   if(count == 'prev'){
    pageNumber.value--;
   }else if(count == 'next'){
    pageNumber.value++;
   }else{
    pageNumber.value = count
   }
   setTimeout(() => {
        window.scrollTo({top: 0, behavior: 'smooth'});
    }, 1)
}

//Open chart modal
const openModal = (data) => {
    store.modalShow = true
    chartLabels.value = data.map(item => {
        return item.email
    })
    chartDataSet.value = 
       data.map(item => {
        return item.email.length
    })
    
}

// filter array posts
watch(
    () => search.value,
    (value) => {
        if (value.trim() !== '') {
            postsArray.value = props.posts.filter(item => item.title.toLowerCase().includes(value.toLowerCase()));
        }else{
            postsArray.value = props.posts
        }
    }
)

let pageCount = computed(() => {
    let l = postsArray.value.length,
        s = size;
      return Math.ceil(l/s);
})
let paginatedData = computed(() => {
    const start = pageNumber.value * size,
          end = start + size;
        return postsArray.value.slice(start, end)
})
let pages = computed(() => {
    let arr = [];
    for (let index = (pageNumber.value-2); index < (pageNumber.value+3); index++) {
        if(index >= 0 && index <= Math.ceil((postsArray.value.length/size) - 1)){
            arr.push(index)
        }
    }
    return arr
})



</script>

<template>
    <div>
        <ChartModal v-if="modalActive" :labels="chartLabels" :dataSet="chartDataSet"/>
        <div class="search">
            <input type="text" placeholder="Search" v-model="search">
        </div>
        <div class="posts-wrapper">
            <div v-for="item in paginatedData" :key="item.id" class="post-block">
                <div class="title">
                    <h2>{{ item.title }}</h2>
                </div>
                <div class="info">
                    <div class="text">
                        <p>{{ item.body }}</p>
                    </div>
                </div>
                <div class="count-comments">
                        Коментарів: {{ item.comments.length }}
                        <button @click="openModal(item.comments)" class="default-btn">Open statistics</button>
                </div>
            </div>
        </div>
        <div class="pagination-button">
                <button @click="switchPage('prev')" :disabled="pageNumber==0">
                    Previous
                </button>
                <div v-for="item in pages" :key="item">
                    <button @click="switchPage(item)" :class="item == pageNumber ? 'active' : ''">
                        {{ item + 1  }}
                    </button>
                </div>
                <button @click="switchPage('next')" :disabled="pageNumber >= pageCount -1">
                    Next
                </button>
        </div>
    </div>
</template>
<style scoped lang="less">
.chart{
    width: 200px;
}
    .posts-wrapper{
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-gap: 20px;
        .post-block{
            background-color: #242424;
            border-radius: 20px;
            padding: 20px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            .title{
                color: hsla(160, 100%, 37%, 1);
            }
            .info{
                flex: 1;
                margin-top: 20px;
            }
            .count-comments{
                margin-top: 20px;
                display: flex;
                justify-content: space-between;
                align-items: center;
            }
        }
    }
    .pagination-button{
        padding: 20px 0;
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        grid-gap: 10px;
        align-items: center;

        button{
            background-color: #242424;
            border-radius: 20px;
            padding: 10px 15px;
            font-size: 16px;
            color: hsla(160, 100%, 37%, 1);
            cursor: pointer;
            border: none;
            box-shadow: 1px 1px 10px 1px rgb(37, 48, 37);

            &:hover{
                background-color: rgb(37, 48, 37);
            }
            &:disabled{
                box-shadow: none;
                color: rgba(0, 189, 126, 0.438);
                background-color: #2424248f;
                &:hover{
                    background-color: #2424248f;
                }
            }
            &.active{
                background-color: rgb(37, 48, 37);;
                color: rgba(0, 189, 126, 0.897);
            }
        }
    }
    .search{
        padding: 10px 5px;
        text-align: end; 

        input{
            padding: 5px 10px;
            border-radius: 7px;
            background-color: #383838;
            border: 0;
            font-size: 14px;
            color: white;
        }
    }
</style>
