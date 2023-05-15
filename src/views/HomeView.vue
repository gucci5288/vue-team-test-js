<template>
    <!--實作開始-->
    <table class="table">
        <thead>
            <tr>
                <th>ID</th>
                <th>image</th>
                <th>description</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in itemsRef">
                <td class="item">
                    {{item.id}}
                </td>
                <td class="item">
                    <img :src="`../images/${item.image}`"
                         @error="onImgError"
                    >
                </td>
                <td class="item">
                    {{item.description}}
                </td>
            </tr>
        </tbody>

        <div @click="swap_shuffle">swap_shuffle</div>
        <div @click="toggleSwiper">toggleSwiper</div>

    </table>
    <!--實作結束-->
</template>


<script setup>
/**
 * 實作開始
 */
import { computed, reactive,ref } from "vue";

import jsonObj from '@/assets/json/items.json'

const items = jsonObj.items
const defaultImgUrl = '../images/default.png'

/**
 * 將靜態物件轉為Ref
 */
const itemsRef = reactive(items)

const onImgError=(evt)=>{
    console.log('onImgError item',evt);
    evt.target.src = defaultImgUrl
    console.log(evt.target);
}

function getRandomInt(max) {
    return Math.floor(Math.random() * max);
}

const swap_shuffle = () => {
    const changes = 100
    for (let i = 0; i< changes ; i++){
        const x = getRandomInt(items.length)
        const y = getRandomInt(items.length)
        const tmp = itemsRef[x]
        itemsRef[x] = itemsRef[y]
        itemsRef[y] = tmp
    }
    console.log(itemsRef);
}

const timer = ref()

const toggleSwiper = ()=>{
    if(timer.value) {
        timer.value?.clearInterval()
    } else {
        timer.value = setInterval(()=>{
            swap_shuffle()
        },3000)
    }
}

/**
 * 實作結束
 */
</script>

<style lang="css">
.table{
    width: 100%;
    border: solid 1px #f2f2f2;
}


.table tr th{
    border: solid 1px #f2f2f2;
}
.table tr td{
    border: solid 1px #f2f2f2;
}

</style>
