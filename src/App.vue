<template>
   <div id="app">
    <h3>Cuenta regresiva</h3>
    <div>
      {{this.timeLeft}}
    </div>
    <ul>
       <li v-for="(time, index) in times" :key="index">
         <button @click.prevent="setTime(time.sec)">
             {{time.display}}
       </button>
       </li>
    </ul>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                selectedTime: 0 ,
                intervalTimer: '',
                minutes: '',
                seconds: '',
                hour: '',
                timeLeft: '00.00',
                endTime: '0',
                times: [
                    {
                        sec: 1800,
                        display: '30 min'
                    },
                    {
                        sec: 600,
                        display: '10 min'
                    },
                    {
                        sec: 300,
                        display: '5 min'
                    },
                    {
                         sec: 60,
                         display: '1 min'
                    },
                    {
                        sec: 3,
                        display: '3 seg'
                    },          
                ]
            }
        },
        methods: {
          setTime(seconds){
              clearInterval(this.intervalTimer);
              this.timer(seconds);
          },
          timer(seconds){
              const now = Date.now();
              const end = now + seconds * 1000;
              this.displayTimeLeft(seconds);

              this.selectedTime = seconds;

              this.displayEndTime(end);
              this.countdown(end);
          },
          countdown(end){
              this.intervalTimer = setInterval(()=>{
                  const secondsLeft = Math.round((end - Date.now()) / 1000);

                  if(secondsLeft === 0 ){
                      this.endTime = 0;
                  }
                  else if(secondsLeft < 0) {
                      clearInterval(this.intervalTimer);
                      return;
                  }
                  this.displayTimeLeft(secondsLeft)
              }, 1000);
          },
          displayTimeLeft(secondsLeft){
              this.minutes = Math.floor((secondsLeft % 3600) / 60 );
              this.seconds = secondsLeft % 60;

              this.timeLeft = `${this.minutes}:${this.seconds}`;
          },
          displayEndTime(timestamp){
            const end = new Date(timestamp);
            this.hour = end.getHours();
            this.minutes = end.getMinutes();

            this.endTime = `${this.hour}:${this.minutes}`
          },
      },
    }
</script>

<style>
button{
  margin-right: 2em;
}
</style>