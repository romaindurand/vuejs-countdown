# vuejs-countdown
A simple countdown timer component for VueJS 2.

![screenshot](https://raw.githubusercontent.com/getanwar/vuejs-countdown/master/scr.png "Vue JS Countdown")

## Installation
#### npm

`npm i vuejs-countdown --save`


## Usage

```html
<template>
  <div>
    <Countdown deadline="August 22, 2022"></Countdown>
    shorthand :
    <Countdown end="August 22, 2022"></Countdown>
    using JSON date format (ie: "2019-01-12T19:44:55.906Z")
    <Countdown :end="new Date().toJSON()"/>
    using custom labels :
    <Countdown end="August 22, 2022" :i18n="countdownLabels"></Countdown>
    finished callback
    <Countdown end="August 22, 2022" @finished="onCountdownEnd"/>
  </div>
</template>
```

```javascript
<script>
import Countdown from 'vuejs-countdown'

export default {
  components: { Countdown },
  data() {
    return {
      countdownLabels: {
        day: ['jour', 'jours'], // when overriding default labels, 
        hour: ['heure', 'heures'] // you must specify both singular and plural form
        // ommiting 'minute' and 'second' keys fallback to default (min/sec)
      }
    }
  },
  methods: {
    onCountdownEnd() {
      console.log('timer finished')
    }
  }
}
</script>
```

## Other Config

You can stop the countdown timer anytime by passing `true` (Boolean) with `stop` props.


### Caution 

Please don't provide any confusing date format since it has no dependency to Moment.js or any other date helpers.