<template>
    <div class="body__list">
        <ul>
            <li v-on:click="allPeriod">Весь срок</li>
            <li v-on:click="presentDay">Сегодня</li>
            <li v-on:click="yesterday">Вчера</li>
            <li v-on:click="lastSevenDays">Последние 7 дней</li>
            <li v-on:click="lastThirtyDays">Последние 30 дней</li>
            <li v-on:click="thisMonth">В этом месяце</li>
            <li v-on:click="previousMonth">Прошлый месяц</li>
        </ul>
    </div>
</template>
<script>


export default {
    props:["allPeriod", "presentDay","yesterday", "lastSevenDays", "lastThirtyDays", "thisMonth", "previousMonth"],
    data() {
        return {
            
        } 
    },
    methods: {
        
    },
    computed: {
       
    },
}
</script>

<style scoped>
    .body__list {
        box-shadow: 0px 10px 40px rgba(128, 158, 191, 0.2);
        border-radius: 10px;
        background-color: #FFFFFF;
        margin-right: 10px;
    }

    ul {
        list-style-type: none;
        padding: 0;
        margin: 0;
        font-family: PT Sans;
        font-size: 15px;
        line-height: 19px;
    }
    li {
        width: 146px;
        padding-top: 8.5px;
        padding-bottom: 8.5px;
        padding-left: 10px;
    }
    li:hover {
        border: 2px solid #EBEEF3;
        border-radius: 5px;
    }
    li:active {
        background-color:  #F0F3F8;
        border-radius: 5px;
    }
    .previous {
       opacity: 0.22;
    }
</style>