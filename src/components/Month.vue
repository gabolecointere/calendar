<template>
  <div class='month-container'>
            <div class='month-header'>
                <div class='days'>
                    <div class='day-legend'>S</div>
                    <div class='day-legend'>M</div>
                    <div class='day-legend'>T</div>
                    <div class='day-legend'>W</div>
                    <div class='day-legend'>T</div>
                    <div class='day-legend'>F</div>
                    <div class='day-legend'>S</div>
                </div>
                <div class='month-year'>{{getMonthName(monthId)}}, {{year}}</div>
            </div>        
            <div class='month-body'>
                <Day v-for='day in days' :key='day.id' :type='day.type' :date='day.date' :day='day.day' :countryCode='countryCode' :monthId='monthId' :year='year' />       
            </div>
        </div>
</template>

<script>
import Mixin from './Mixin'
import Day from './Day'

export default {
    props: {
        start: {
            type: String,
            required: true
        },
        end: {
            type: String,
            required: true
        },
        monthId: {
            type: Number,
            required: true
        },
        year: {
            type: Number,
            required: true
        },
        countryCode: {
            type: String,
            required: true
        }
    },

    mixins: [Mixin],

    components: {
        Day
    },

    data() {
        return {
            holidays: []
        }
    },

    computed: {
        days() {
            return this.calculateDays(this.monthId, new Date(this.end), new Date(this.start))
        }
    },

    methods: {
        calculateDays (id, end, start) {
            const finalValidDate = end 
            const year = finalValidDate.getFullYear()
            const startValidDate = this.monthComparator(end, start) ? start : new Date(year, id, 1) 
            const finalWeek = this.getFinalWeek(finalValidDate) 
            let currentDate = this.getBeginWeek(startValidDate) 
            const days = []
            let cont = 1

            while(!this.dateComparator(currentDate,finalWeek)) {
                if(this.dateIsBeetween(currentDate, startValidDate, finalValidDate)){
                    if(this.getDayName(currentDate.getDay()) === "Sun" || this.getDayName(currentDate.getDay()) === "Sat"){
            
                        days.push({id:cont, type:"weekend", day:currentDate.getDay(), date: currentDate.getDate()})
                        cont += 1
                     currentDate = this.sumDays(currentDate)
                
                    } else {
                        days.push({id:cont, type:"green", day:currentDate.getDay(), date: currentDate.getDate()})
                        cont += 1
                        currentDate = this.sumDays(currentDate)
                    }
                } else {
                    days.push({id:cont, type:"gray", day:currentDate.getDay(), date: currentDate.getDate()})
                    cont += 1
                    currentDate = this.sumDays(currentDate)
                }
            }
    
            return days
        },

        getBeginWeek(d) {
            return (new Date(d.getFullYear(), d.getMonth(), (d.getDate()-d.getDay())))
        },

        getFinalWeek(d) {
            const difference = (6 - d.getDay()) + 1
            return this.sumDays(d, difference)
        },

        dateIsBeetween(c,s,e) {
            let isBeeetween = true

            if(c.getMonth() !== s.getMonth() || c.getDate() < s.getDate() || c.getMonth() !== e.getMonth() || c.getDate() > e.getDate() ) isBeeetween = false

            return isBeeetween
        },

        getDayName(id)  {
            const dayNames = ["Sun", "Mon", "Tus", "Wed", "Thu","Fri","Sat"]
            return dayNames[id]
        },

        getMonthName(id) {
            const monthNames = ["Jan","Feb","Mar","Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];
            return monthNames[id];
        }
    }
}
</script>

<style scoped>
.month-container, .month-header {

    display:flex;
    flex-direction: column;
}

.month-container {
    width: 220px;
    margin: 10px;
}

.month-body {

    display: flex;
    flex-wrap: wrap;
    width: 220px;
}

.month-year {

    text-align: center;
    font-size: 12px;
    font-weight: bolder;
    background-color: #b1b1b1;
    padding: 3px 0;
    margin-right: 4px;
}

.days {

    display: flex;
    justify-content: space-between;
}

.days {

    font-size: 12px;
    font-weight: bolder;
    color: #535353;
    margin-right: 4px;
}

.day, .day-legend {

    width: 30px;
    height: 30px;
    margin-right: 1px;
    margin-top: 1px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.day {
    font-size: 13px;
}

.day.gray {

    background-color: #e3e3e3;
}
.day.gray:after {

    content: " ";
}

.day.green {

    background-color: #1d9e1d;
}

.day.weekend {

    background-color: #eeee52;
}



</style>


