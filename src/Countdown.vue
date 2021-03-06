<template>
    <ul class="vuejs-countdown">
        <li v-if="days > 0">
            <p class="digit">{{ days | twoDigits }}</p>
            <p class="text">{{ days > 1 ? labels.day[1] : labels.day[0] }}</p>
        </li>
        <li>
            <p class="digit">{{ hours | twoDigits }}</p>
            <p class="text">{{ hours > 1 ? labels.hour[1] : labels.hour[0] }}</p>
        </li>
        <li>
            <p class="digit">{{ minutes | twoDigits }}</p>
            <p class="text">{{ minutes > 1 ? labels.minute[1] : labels.minute[0] }}</p>
        </li>
        <li>
            <p class="digit">{{ seconds | twoDigits }}</p>
            <p class="text">{{ seconds > 1 ? labels.second[1] : labels.second[0] }}</p>
        </li>
    </ul>
</template>

<script>

export default {
    name: 'vuejsCountDown',
    props: {
        deadline: {
            type: String
        },
        end: {
            type: String
        },
        stop: {
            type: Boolean
        },
        i18n: {
            type: Object
        }
    },
    data() {
        return {
            now: Math.trunc((new Date()).getTime() / 1000),
            date: null,
            diff: 0,
            interval: null,
            labels: {
                day: (this.$props.i18n && this.$props.i18n.day) || ['day', 'days'],
                hour: (this.$props.i18n && this.$props.i18n.hour) || ['hour', 'hours'],
                minute: (this.$props.i18n && this.$props.i18n.minute) || ['min', 'min'],
                second: (this.$props.i18n && this.$props.i18n.second) || ['Sec', 'Sec']
            }
        }
    },
    created() {
        if (!this.deadline && !this.end) {
            throw new Error("Missing props 'deadline' or 'end'");
        }

        let endTime = this.deadline ? this.deadline : this.end;
        this.date = Math.trunc(Date.parse(endTime.replace(/-/g, "/")) / 1000);

        if (!this.date) {
            this.date = +new Date(endTime) / 1000
        }

        if (!this.date) {
            throw new Error("Invalid props value, correct the 'deadline' or 'end'");
        }
    },
    mounted() {
        this.interval = setInterval(() => {
            this.now = Math.trunc((new Date()).getTime() / 1000);
        }, 1000);
    },
    computed: {
        seconds() {
            return Math.trunc(this.diff) % 60
        },

        minutes() {
            return Math.trunc(this.diff / 60) % 60
        },

        hours() {
            return Math.trunc(this.diff / 60 / 60) % 24
        },

        days() {
            return Math.trunc(this.diff / 60 / 60 / 24)
        }
    },
    watch: {
        now(value) {
            this.diff = this.date - this.now;
            if(this.diff <= 0 || this.stop){
                if (this.diff <= 0) this.$emit('finished');
                this.diff = 0;
                // Remove interval
                clearInterval(this.interval);
            }
        }
    },
    filters: {
        twoDigits(value) {
            if ( value.toString().length <= 1 ) {
                return '0'+value.toString()
            }
            return value.toString()
        }
    },
    destroyed() {
        clearInterval(this.interval);
    }
}
</script>
<style>
.vuejs-countdown {
  padding: 0;
  margin: 0;
}
.vuejs-countdown li {
  display: inline-block;
  margin: 0 8px;
  text-align: center;
  position: relative;
}
.vuejs-countdown li p {
    margin: 0;
}
.vuejs-countdown li:after {
  content: ":";
  position: absolute;
  top: 0;
  right: -13px;
  font-size: 32px;
}
.vuejs-countdown li:first-of-type {
  margin-left: 0;
}
.vuejs-countdown li:last-of-type {
  margin-right: 0;
}
.vuejs-countdown li:last-of-type:after {
  content: "";
}
.vuejs-countdown .digit {
  font-size: 32px;
  font-weight: 600;
  line-height: 1.4;
  margin-bottom: 0;
}
.vuejs-countdown .text {
  text-transform: uppercase;
  margin-bottom: 0;
  font-size: 10px;
}
</style>
