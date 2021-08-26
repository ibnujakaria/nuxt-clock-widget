<template>
  <div class="container">
    <div id="clock">
      <div class="hour"> 
        <div class="min"></div>
        <div class="min"></div>
        <div class="min"></div>
        <div class="min"></div>
        <div class="min"></div>
      </div>
      <div class="hour">
        <div class="min"></div>
        <div class="min"></div>
        <div class="min"></div>
        <div class="min"></div>
        <div class="min"></div>
      </div>
      <min ref="min"></min>
      <hour ref="hour"></hour>
      <sec ref="sec"></sec>
      <ol>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ol>
      <time :my-message="time"></time>
    </div>
  </div>
</template>

<script>
import Min from './Min.vue'
import Hour from './Hour.vue'
import Sec from './Sec.vue'
import Time from './Time.vue'

export default {
  components: {
    Min,
    Hour,
    Sec,
    Time,
  },
  data: () => ({
    time: "00:00:00",
  }),
  mounted () {
    this.startTime()
  },
  methods: {
    startTime: function () {
      var min = this.$refs.min.$el,
      sec = this.$refs.sec.$el,
      hour = this.$refs.hour.$el;
      var now = new Date(),
      hValue = now.getHours(),
      mValue = now.getMinutes(),
      sValue = now.getSeconds(),
        
      then = new Date(now.getFullYear(),now.getMonth(),now.getDate(),0,0,0),//midnight
      diffInMil = (now.getTime() - then.getTime()),// difference in milliseconds
      h = (diffInMil/(1000*60*60)),//hours
      m = (h*60);//minutes

      //rotate the hands accordingly
      sec.style.webkitTransform = "rotate(" + (sValue * 6) + "deg)";
      hour.style.webkitTransform = "rotate(" + (h * 30) + "deg)";
      min.style.webkitTransform = "rotate(" + (m * 6) + "deg)";
      
      setTimeout(this.startTime, 1000);

      // 数字时间
      mValue = this.checkTime(mValue);
      sValue = this.checkTime(sValue);
      this.time = hValue + ":" + mValue + ":" + sValue;
    },
    checkTime: function (i) {
      if (i < 10) {
        i = "0" + i
      };
      return i;
    }
  }
}
</script>

<style lang="scss">
@mixin center {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}

@mixin before-after {
  display: block;
  content: "";
  position: absolute;
}

@mixin before-after-tick {
  display: block;
  content: "";
  position: absolute;
  height: inherit;
  width: inherit;
  background: inherit;
  box-shadow: inherit;
  backface-visibility: inherit;
}

*,
*:before,
*:after {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  font-size: 100%;
  font-family: helvetica;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: linear-gradient(to right, #bfbba9 0%, #6e6e6e 100%);
}

$tick: #383838;
$sec: #fbc900;
$min-hour: #303030;
$watch-body: #fff;
#clock {
  @include center;
  width: 21em;
  height: 21em;
  border-radius: 50%;
  background: #eee;
  border: 0.2em solid #d0d0d0;
  box-shadow: 0 1.2em 0 -1em rgba(255, 255, 255, 1), 0 -0.1em 0.3em 0 #fff,
    0 0 0 0.6em #e7e7e7, 0 0.6em 1.2em 0 rgba(0, 0, 0, 0.5),
    inset 0 0.5em 1em 0 rgba(0, 0, 0, 0.3), inset 0 0 8em 0 rgba(0, 0, 0, 0.2);
  &:before {
    @include before-after;
    background: #f1a432;
    left: -2em;
    top: -2em;
    width: 25em;
    height: 25em;
    border-radius: 4.6em;
    z-index: -1;
    box-shadow: inset 0 -0.2em 0.2em 0 rgba(0, 0, 0, 0.2),
      inset 0 0.2em 2em 0 #fff;
  }
  &:after {
    @include before-after;
    height: inherit;
    width: inherit;
    background: transparent;
    top: 0;
    left: 0;
    border-radius: inherit;
    box-shadow: inset 12em 14em 0 -10em rgba(255, 255, 255, 0.25);
    z-index: 4;
  }
  // Hours
  .hour {
    @include center;
    width: 0.5em;
    height: 16.4em;
    background: transparent;
    box-shadow: 0 -1em 0 -0.1em $tick, 0 1em 0 -0.1em $tick;
    transform: rotate(30deg);
    &:after,
    &:before {
      @include before-after-tick;
    }
    &:before {
      transform: rotate(30deg);
    }
    &:after {
      transform: rotate(-30deg);
    }
    &:nth-of-type(1) {
      transform: rotate(-60deg);
    }
  }
  // Minutes
  .min {
    @include center;
    width: 0.3em;
    height: 17em;
    background: transparent;
    box-shadow: 0 -0.6em 0 -0.1em $tick, 0 0.6em 0 -0.1em $tick;
    z-index: -1;
    transform: rotate(-54deg);
    &:after,
    &:before {
      @include before-after-tick;
      z-index: 0;
    }
    &:before {
      transform: rotate(6deg);
    }
    &:after {
      transform: rotate(12deg);
    }
    &:nth-child(2) {
      transform: rotate(-36deg);
    }
    &:nth-child(3) {
      transform: rotate(-18deg);
    }
    &:nth-child(4) {
      transform: rotate(6deg);
    }
    &:nth-child(5) {
      transform: rotate(24deg);
    }
  }
}

// Second Hand
#sec {
  @include center;
  background: radial-gradient(
    ellipse at center,
    rgba(255, 211, 34, 1) 0%,
    rgba(251, 201, 0, 1) 100%
  );
  width: 2em;
  height: 2em;
  border-radius: 50%;
  border: 0.14em solid $sec;
  z-index: 3;
  &:before,
  &:after {
    @include before-after;
  }
  &:before {
    background: $sec;
    width: 0.8em;
    height: 1.5em;
    top: 1.8em;
    left: 0;
    right: 0;
    margin: 0 auto;
    border-radius: 0 0 2em 2em;
  }
  &:after {
    width: 0.18em;
    height: 7.6em;
    top: -7.6em;
    background: $sec;
    left: 0;
    right: 0;
    margin: 0 auto;
  }
}

// Minute and Hour Hands
#min,
#hour {
  z-index: 2;
  @include center;
  background: $sec;
  transform-origin: bottom center;
}

#min {
  width: 0.6em;
  height: 8.6em;
  top: -8.6em;
  border-radius: 2em 2em 0 0;
  box-shadow: inset 0 0 0 0.16em $min-hour, inset 0 -6em 0 0 $min-hour,
    -0.2em -0.2em 0.4em 0 rgba(0, 0, 0, 0.2);
}

#hour {
  width: 0.7em;
  height: 7em;
  top: -7em;
  border-radius: 2em 2em 0 0;
  box-shadow: inset 0 0 0 0.16em $min-hour, inset 0 -4.2em 0 0 $min-hour,
    -0.2em -0.2em 0.4em 0 rgba(0, 0, 0, 0.2);
}

// electronic time
#time {
  line-height: 30px;
  font-size: 20px;
  text-align: center;
  position: absolute;
  box-shadow: 1px 2px 10px 3px rgba(31, 24, 21, 0.15),
    1px 1px 0px 0px rgba(0, 0, 0, 0.43);
  background: linear-gradient(
    to bottom,
    rgba(51, 48, 40, 0.04) 0%,
    rgba(255, 255, 255, 0) 40%,
    rgba(78, 41, 41, 0.26) 60%,
    rgba(255, 255, 255, 0.15) 100%
  );
  border: 1px solid rgba(0, 0, 0, 0);
  border-bottom: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 4px;
  width: 100px;
  height: 30px;
  right: 0;
  left: 0;
  bottom: 27%;
  margin: 0 auto;
  color: #714c14;
  &:after {
    content: "";
    position: absolute;
    width: 100px;
    height: 30px;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    box-shadow: inset 0 0 20px 0px rgba(255, 255, 255, 1);
    transition: all 0.5s;
  }
}

ol {
  position: relative;
  height: inherit;
  width: inherit;
  margin: 0;
  padding: 0;
  li {
    counter-increment: customlistcounter;
    font-size: 1.4rem;
    display: inline-block;
    position: absolute;
    letter-spacing: -0.1em;
    color: $tick;
    text-align: center;
    &:before {
      content: counter(customlistcounter) "";
    }
    &:nth-child(1),
    &:nth-child(2),
    &:nth-child(4),
    &:nth-child(5),
    &:nth-child(7),
    &:nth-child(8),
    &:nth-child(10),
    &:nth-child(11) {
      font-size: 0;
    }
    &:nth-child(3) {
      top: 6.95em;
      right: 2.4em;
    }
    &:nth-child(9) {
      top: 6.95em;
      left: 2em;
    }
    &:nth-child(6) {
      bottom: 2.1em;
      left: 7.06em;
    }
    &:nth-child(12) {
      top: 1.9em;
      left: 6.78em;
    }
  }
}
</style>