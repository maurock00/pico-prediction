<template>
<div>
    <section class="row">
        <div class="small-6 columns">
            <form> 
                <label for="plateNumber"> Plate Number </label>
                <input type="text" name="plateNumber" id="plateNumber" 
                    v-model="plateNumber">
                
                <label for="plateNumber"> Date </label>
                <input type="date" name="dateOf" id="dateOf" 
                    v-model="dateOf">
                
                <label for="timeOf"> Time </label>
                <input type="time" name="timeOf" id="timeOf" 
                    v-model="timeOf">   

                <button
                    @click.prevent="calculateResult"> Predict !
                </button>   
            </form>
            <hr>
        </div>
    </section>
        <div v-if="appLog.length >=1" class="log    ">
            <h1> Results Log </h1>
                <ul>
                    <li v-for="item in appLog" :class="{ 'allowed': item.canTravel, 'prohibited': !item.canTravel}">
                        {{ item.text }}
                    </li>
                </ul>
        </div>
</div>
</template>


<script>
export default {
    data() { 
        return { 
            plateNumber: '',
            dateOf: '', 
            timeOf: '', 
            appLog: []
        }
    }, 
    methods: { 
        calculateResult() { 
            var lastDigit = this.extractLastDigit(this.plateNumber);
            var dayOf = this.getDayOfWeek(this.dateOf);

           // console.log(this.getDayOfWeek(this.dateOf));
           // console.log(this.timeToString(this.timeOf));

            var intTime = parseInt(this.timeToString(this.timeOf)); 

            //console.log(intTime );

            if( dayOf >= 1 && dayOf <= 5 ) {
                if( ( intTime >= 700 && intTime <= 930 ) || 
                    ( intTime >= 1600 && intTime <= 1930 ) ) { 
                        switch ( dayOf == 1 && (lastDigit == 1 || lastDigit == 2) ) {
                            case true :
                                this.appLog.unshift( { canTravel: false, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' cannot be on the road ' } ); 
                                break; 
                            case 2 :
                                this.appLog.unshift( { canTravel: false, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' cannot be on the road ' } );  
                                break;
                            case 3 :
                                this.appLog.unshift( { canTravel: false, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' cannot be on the road ' } ); 
                                break;
                            case 4 :
                                this.appLog.unshift( { canTravel: false, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' cannot be on the road ' } ); 
                                break;
                            case 5 :
                                this.appLog.unshift( { canTravel: false, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' cannot be on the road ' } ); 
                                break;
                            default: 
                                this.appLog.unshift( { canTravel: true, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' can be on the road ' } );
                        } 
                } else { 
                    this.appLog.unshift( { canTravel: true, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' can be on the road ' } );
                }
            }
            else { 
                this.appLog.unshift( { canTravel: true, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' can be on the road ' } );
            }
        }, 
        extractLastDigit(inputNumber) { 
            var lastDigit = ''; 
            var digitsArray = inputNumber.split(''); 

            //console.log(digitsArray[digitsArray.length-1]);
            return digitsArray[digitsArray.length-1];
        }, 
        getDayOfWeek(inputDate) { 
            var parts = inputDate.split('-'); 

            var parsedDate = new Date(parts[0], parts[1] - 1, parts[2]); 
            //console.log(parsedDate);

            //console.log(parsedDate.getDay()); 

            var dayOfWeek = parsedDate.getDay();

            if (dayOfWeek == 0)
                return 7; 
            else
                return dayOfWeek;
        }, 
        timeToString(inputTime) { 
            var timeParts = inputTime.split(':'); 

            return timeParts[0]+''+timeParts[1];
        }
    }
} 
</script>

<style>

.log ul .allowed {
    color: blue;
    background-color: #e4e8ff;
    margin: .5rem;
}

.log ul .prohibited {
    color: red;
    background-color: #ffc0c1;
    margin: .5rem;
}

</style>
