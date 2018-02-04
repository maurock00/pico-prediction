<template>
 <div class="container">
        <form>
            <div class="row">
                <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                    <h1 class="text-center">"Pico y placa" predictor</h1>
                </div>
                <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 framed">
                    <div class="form-group">
                        <label for="plateNumber">Plate number</label>
                        <input
                                type="text"
                                id="plateNumber"
                                class="form-control"
                                v-model="plateNumber">
                    </div>
                    <div class="form-group">
                        <label for="dateOf">Date</label>
                        <input
                                type="date"
                                id="dateOf"
                                class="form-control"
                                v-model="dateOf">
                    </div>
                    <div class="form-group">
                        <label for="timeOf">Time</label>
                        <input
                                type="time"
                                id="timeOf"
                                class="form-control"
                                v-model="timeOf">
                    </div>

                </div>
            </div>
            <hr>
            <div class="row">
                <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 text-center">
                    <button
                            class="btn btn-primary"
                            @click.prevent="calculateResult">Predict!
                    </button>
                </div>
            </div>
        </form>
        <hr>
        <section class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 log framed" v-show=" appLog.length >=1 ">
            <h2 class="text-center"> Results of your submits </h2>
            <div class="small-12 columns">
                <ul>
                    <li v-for="item in appLog" :class="{ 'allowed': item.canTravel, 'prohibited': !item.canTravel }">   
                        {{ item.text }}
                    </li>
                </ul>
            </div>
        </section>
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

<style scoped>

.log ul li {
    padding: 3px;
    margin: 5px;
}

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

.text-center {
    text-align: center;
}

.framed { 
    
    border: solid .5px;
    border-color: lightgrey;
}

</style>
