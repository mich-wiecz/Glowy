<template>
        <Star 
        class="star"
        :class="[{falling: falling}, getDuration()]"
        :style="{transform: `rotate(${rotation}deg)`}"
        :branches="noOfBranches"
        :color="noOfBranches === 0 ? 'rgb(21, 69, 202)' : 'rgb(26, 96, 177)'"
        :width="width"
        :creationRate="creationRate"
        />
</template>

<script>
import Star from './Star'
export default {
    components: {
        Star
    },
    props: {
        width: {
            type: Number,
            required: true
        }
    },
    data() {
        return {
            timeoutId: null,
            falling: false,
            noOfBranches: 0,
            translation: 0,
            creationRate: Math.random(),
            rotation: Math.floor(Math.random() * 361),
            maxNonactivity: 1000 * 60 * 2,
            timeForCreation: 500
        }
    },
    methods: {
        getDuration() {
            const durations = [5, 7, 10, 12, 15]
            const index =  Math.floor(Math.random() * durations.length)
            return 'duration-' + durations[index].toString()
        },
        waitForFalling() {
          this.timeoutId = setTimeout(() => {
              this.falling = true
           }, this.timeForCreation)
        }
    },
    mounted() {
       this.timeoutId =  setTimeout(() => {
           this.noOfBranches = 1 + Math.floor(Math.random() * 3)

           if (this.noOfBranches === 1 && Math.random() > .5) {
             this.timeoutId = setTimeout(() => {
                 this.noOfBranches = 2 + Math.floor(Math.random() * 2)
                 this.falling = true
             }, 2000)   
           } else {
                this.waitForFalling()   
           }     

       }, Math.floor(Math.random() * this.maxNonactivity))
    },
    unmounted() {
        if (this.timeoutId)
            clearTimeout(this.timeoutId)
    }
}
</script>

<style>

 .falling {
     animation: fall ease-in-out forwards;
 }

 .duration-5 {
     animation-duration: 5s;
 }
 .duration-7 {
     animation-duration: 7s;
 }
 .duration-10 {
     animation-duration: 10s;
 }
 .duration-12 {
     animation-duration: 12s;
 }
 .duration-15 {
     animation-duration: 15s;
 }

 @keyframes fall {
     to {
          transform: translateY(1000px)
     }
 }
</style>