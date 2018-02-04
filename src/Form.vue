<template>
 <div class="container">
        <!-- The form to get input from users -->
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
                                maxlength="7"
                                placeholder="AAA0000"
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
        <!-- A log which shows the results of the predictions -->
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
        // Data which the app uses to calculate the prediction
        return { 
            plateNumber: '',
            dateOf: '', 
            timeOf: '', 
            appLog: []
        }
    }, 
    methods: { 
        //The method to calculate the prediction
        calculateResult() { 

            //Simple input control 
            if ( !(this.plateNumber == '' || this.dateOf == '' || this.timeOf == '') )
            {
                // Vairables which use other methods to trasform user data into values that app uses to calculate the prediction 
                var lastDigit = this.extractLastDigit(this.plateNumber);
                var dayOf = this.getDayOfWeek(this.dateOf);
                var intTime = parseInt(this.timeToString(this.timeOf)); 

                //Decides if the car can be on the road according to the day of the week
                if( dayOf >= 1 && dayOf <= 5 ) {

                    //Decides if the car can be on the road according on the time of the day
                    if( ( intTime >= 700 && intTime <= 930 ) || 
                        ( intTime >= 1600 && intTime <= 1930 ) ) { 
                            
                            //Decides if the car can be on the road according on the last digit of the plate
                            if ( ( dayOf == 1 && (lastDigit == 1 || lastDigit == 2) ) ||
                                 ( dayOf == 2 && (lastDigit == 3 || lastDigit == 4) ) ||
                                 ( dayOf == 3 && (lastDigit == 5 || lastDigit == 6) ) ||
                                 ( dayOf == 4 && (lastDigit == 7 || lastDigit == 8) ) ||
                                 ( dayOf == 5 && (lastDigit == 9 || lastDigit == 0) ) )

                            {  
            // Following lines just add the results to the appLog array
                                this.appLog.unshift( { canTravel: false, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' cannot be on the road ' } ); 1
                            } else 
                                this.appLog.unshift( { canTravel: true, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' can be on the road ' } ); 
                    } else { 
                        this.appLog.unshift( { canTravel: true, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' can be on the road ' } );
                    }
                }
                else { 
                    this.appLog.unshift( { canTravel: true, text: ' The car with plate ' + this.plateNumber + ' on ' + this.dateOf + ' at ' + this.timeOf + ' can be on the road ' } );
                }
            } else 
            { 
                alert('Please fill all the inputs to get a result '); 
            }
        }, 
        //A method to extract the last digit of a number
        extractLastDigit(inputNumber) { 
            var lastDigit = ''; 
            var digitsArray = inputNumber.split(''); 
            return digitsArray[digitsArray.length-1];
        }, 
        //A method to extract the day of the week of a date
        getDayOfWeek(inputDate) { 
            var parts = inputDate.split('-'); 
            var parsedDate = new Date(parts[0], parts[1] - 1, parts[2]); 
            var dayOfWeek = parsedDate.getDay();

            //A simple control to return 7 if the day is Sunday 
            if (dayOfWeek == 0)
                return 7; 
            else
                return dayOfWeek;
        }, 
        //A method to convert the time into a string variable
        timeToString(inputTime) { 
            var timeParts = inputTime.split(':'); 

            return timeParts[0]+''+timeParts[1];
        }
    }
} 
</script>

<style scoped>

/* Some styles to make the app looks nice */

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
