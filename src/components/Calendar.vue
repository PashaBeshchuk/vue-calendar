<template>
  <div class="body">
    <Days
        v-bind:allPeriod = "allPeriod"
        v-bind:presentDay = "presentDay"
        v-bind:yesterday = "yesterday"
        v-bind:lastSevenDays = "lastSevenDays"
        v-bind:lastThirtyDays = "lastThirtyDays"
        v-bind:thisMonth = "thisMonth"
        v-bind:previousMonth = "previousMonth"
    />
    <div >
        <div class="body__table">
            <div class="date"> 
                <button v-on:click="decrease">
                    <i class="arrow left"></i>
                </button>
                <span>{{monthes[month]}},{{year}}</span>
                <button v-on:click="increase">
                    <i class="arrow right"></i>
                </button>
            </div>
            <table>
                <thead>
                    <tr>
                        <th v-for="(d, index) of day" v-bind:key=index >{{d}}</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(week, index) in days" v-bind:key=index> 
                        <td 
                            v-for="(day, index) in week" 
                            v-bind:key=index 
                            v-on:click="updateStateDate(day)"
                            v-bind:class="{active: day.active, previous: day.previous}">
                            
                            {{day.index}}
                        </td>
                    </tr>            
                </tbody>
            </table>
            <button class="btn cencel" v-on:click="cencel">Отмена</button>
            <button class="btn update"
                v-bind:class="{activeButton: activeButtonUpdate}"
                v-on:click="update"
            >Обновить</button>
        </div>
    </div>
  </div>
</template>

<script>

import Days from './Days.vue'
export default {
    mounted(){
        this.calendar()
    },
    data() {
        return {
            month: new Date().getMonth(),    
            year: new Date().getFullYear(), 
            dFirstMonth: '1',
            day:["Пн", "Вт","Ср","Чт","Пт","Сб", "Вс"],
            monthes:["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"],
            date: new Date(),
            days:null,
            checkDays:[],
            activeButtonUpdate: false,
            firstDay: null,
            lastDay: null,

        } 
    },
    components: {
        Days
    },
    methods: {
        calendar() {
            let days = [];
            let week = 0;
            days[week] = [];
            let a
            let dlast = new Date(this.year, this.month + 1, 0).getDate();
            let dlastPrevious = new Date(this.year, this.month + 0, 0).getDate();
            let nextDay = 1
            let previousDay = 0 
            for (let i = 1; i <= dlast; i++) {
                if (new Date(this.year, this.month, i).getDay() != this.dFirstMonth) {
                    a = {index:i, previous: false, active: false};
                    days[week].push(a);
                } else {
                    week++;
                    days[week] = [];
                    a = {index:i, previous: false, active: false};
                    days[week].push(a);
                }
            }
            if (days[0].length > 0) {
                for (let i = days[0].length; i < 7; i++) {
                    days[0].unshift('');
                }
            }
            for(let j = 0; j < days[0].length; j++ ) {
                if(days[days.length-1][j] === "" || !days[days.length-1][j] ) {
                    days[days.length-1][j] = {index: nextDay, previous: true, active: false}
                    nextDay++
                }
                if(days[0][6-j] === "") {
                    days[0][6-j] = {index: dlastPrevious - previousDay, previous: true, active: false}
                    previousDay++
                }
            }
            this.days = days
            return days;
        },
        decrease() {
            this.month--;
            if (this.month < 0) {
                this.month = 12;
                this.month--;
                this.year--;
            }
            this.calendar()
        },
        increase() {
            this.month++;
            if (this.month > 11) {
                this.month = -1;
                this.month++;
                this.year++;
            }
            this.calendar()
        },
        update(){
            if(this.activeButtonUpdate) {
                alert(`${this.firstDay} ${this.lastDay ? - this.lastDay: ""} 0${this.month} ${this.year}`)
            }
        },
        cencel(){
            for(let i = 0; i < this.days.length; i++) {
                for(let j = 0; j < this.days[i].length; j++) {
                    this.days[i][j].active = false
                }
            }
            this.checkDays=[]
            this.activeButtonUpdate = false
        },
        updateStateDate(day){
            this.firstDay = day.index
            this.activeButtonUpdate = true
            day.active = true
            this.checkDays.push(day)
            if(this.checkDays.length === 2) {
                this.pointAllDate(this.checkDays)  
            }
        },
        pointAllDate(arrDays) {
            this.firstDay = arrDays[0].index
            this.lastDay = arrDays[1].index
            this.activeButtonUpdate = true          
            for(let i = 0; i < this.days.length; i++) {
                for(let j = 0; j < this.days[i].length; j++) {
                    if(this.days[i][j].index >= arrDays[0].index && this.days[i][j].index <= arrDays[1].index) {
                        this.days[i][j].active = true  
                    }
                }
            }
        },
        allPeriod() {
            this.firstDay = this.days[0][0].index
            this.lastDay = this.days[6][6].index
            this.activeButtonUpdate = true
            for(let i = 0; i < this.days.length; i++) {
                for(let j = 0; j < this.days[i].length; j++) {
                    this.days[i][j].active = true
                }
            }
        },
        presentDay() {
            this.activeButtonUpdate = true
            let presentDay = new Date().getDate()
            this.firstDay = presentDay
            for(let i = 0; i < this.days.length; i++) {
                for(let j = 0; j < this.days[i].length; j++) {
                    if(this.days[i][j].index === presentDay){
                        this.days[i][j].active = true
                    } 
                }
            }
        },
        yesterday() {
            this.activeButtonUpdate = true
            let yesterday = new Date().getDate() - 1
            this.firstDay = yesterday
            for(let i = 0; i < this.days.length; i++) {
                for(let j = 0; j < this.days[i].length; j++) {
                    if(this.days[i][j].index === yesterday){
                        this.days[i][j].active = true
                    } 
                }
            }
        },
        lastSevenDays() {
            this.activeButtonUpdate = true
            let toDay = new Date().getDate()
            let stopFunc = false
            this.firstDay = toDay-6
            this.lastDay = toDay
            for(let i = 0; i < this.days.length; i++) {
                if(!stopFunc) {
                    for(let j = 0; j < this.days[i].length; j++) {
                        if(this.days[i][j].index >= toDay-6 && this.days[i][j].index <= toDay){
                            this.days[i][j].active = true
                            if(this.days[i][j].index === toDay) {
                                stopFunc = true
                            }
                        } 
                    }
                }
                
            }
        },
        lastThirtyDays() {
            this.activeButtonUpdate = true
            let toDay = new Date().getDate()
            let stopFunc = false
            this.firstDay = toDay-30
            this.lastDay = toDay
            for(let i = 0; i < this.days.length; i++) {
                for(let j = 0; j < this.days[i].length; j++) {
                    if(i === 0) {
                        this.days[i][j].active = true
                    }
                    if(this.days[i][j].index <=  toDay) {
                        this.days[i][j].active = true
                        if(this.days[i][j].index === toDay) {
                            stopFunc = true
                        }
                    }
                }
            }  
        },
        thisMonth() {
            this.activeButtonUpdate = true
            let dlast = new Date(this.year, this.month + 1, 0).getDate();
            let firstDayMonth = 1
            this.firstDay = firstDayMonth
            this.lastDay = dlast
            for(let i = 0; i < this.days.length; i++) {
                for(let j = 0; j < this.days[i].length; j++) {
                    if(this.days[i][j].index >= firstDayMonth && this.days[i][j].index <= dlast){
                        this.days[i][j].active = true
                    }
                }
            }
        },
        previousMonth() {
            this.activeButtonUpdate = true
            let firstDayMonth = 1
            for(let i = 0; i < this.days[0].length; i++) {
                if(this.days[0][i].index === firstDayMonth) {
                    return
                }
                if(i === 0) {
                    this.firstDay = this.days[0][i].index 
                }
                if(this.days[0][i].index > firstDayMonth) {
                    this.days[0][i].active = true
                    this.lastDay = this.days[0][i].index 
                }
            }
        }
    },
}
</script>

<style scoped>
    .body {
        display: flex;
    }
    .body__table {
        background-color: #FFFFFF;
        border-radius: 10px;
        height: 332px;
        box-shadow: 0px 10px 40px rgba(128, 158, 191, 0.2);
    }
    table {
        width: 288px;
        height: 210px;
        background: #F3F8FD;
        text-align: center;
        margin-top: 10px;
        border-spacing: 0px;
    }
    tr {
        font-size: 12px;
        cursor: pointer;
    }
    td:hover {
        background: #DFE9F3;
    }
    .fone {   
        width: 2em;
        height: 2em;
        border-radius: 50%;
        line-height: 2em;
        text-align: center;
    }
    .active {
        background: rgba(255, 116, 57, 0.2);
        color: white;
    }
    
    .date {
        margin: 0 auto;
        width: 255px;
        padding-top: 15px;
        display: flex;
        justify-content: space-between;
    }
    .date > button {
        width: 26px;
        height: 26px;
        border-radius: 110px;
        border-color: black;
        background-color: white;
        outline: none;
    }
    .btn {
        width: 130px;
        height: 45px;
        border-radius: 7px;
        margin-left: 10px;
        margin-top: 10px;
        font-family: Montserrat;
        font-style: normal;
        font-weight: 600;
        font-size: 12px;
        line-height: 150%;
        outline: none;
    }
    i {
        border: solid black;
        border-width: 0 3px 3px 0;
        display: inline-block;
        padding: 3px;
    }
    .right {
        transform: rotate(-45deg);
        -webkit-transform: rotate(-45deg);
    }
    .left {
        transform: rotate(135deg);
        -webkit-transform: rotate(135deg);
    }
    .cencel {
        background-color: white;
    }
    .update {
        background: rgba(255, 116, 57, 0.5);
        border: none;
        color: white;
    }
    .update:hover {
        background: #F16E36;
        color: white;
    }
    .activeButton{
        background: #FF7439;
        color: white;
    }
    .cencel:hover {
        background: #000000;
        color: white;
    }
    .previous {
       opacity: 0.22;
    }
</style>