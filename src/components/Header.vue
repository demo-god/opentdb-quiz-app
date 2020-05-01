<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>Quiz App</b></b-nav-item>
      <b-nav-item disabled>Counter: {{numCorrect}}/{{numTotal}}</b-nav-item>
    </b-nav>
  
    <!-- <b-progress :class="bar" :value="numCorrect" :max="numTotal" show-value animated></b-progress> -->
    <!-- <b-progress class="mt-2" :max="max" show-value>
      <b-progress-bar :value="value * (6 / 10)" variant="success"></b-progress-bar>
      <b-progress-bar :value="value * (2.5 / 10)" variant="warning"></b-progress-bar>
      <b-progress-bar :value="value * (1.5 / 10)" variant="danger"></b-progress-bar>
    </b-progress> -->
  </div>
</template>

<script>
export default {
  props:{
    numTotal: Number,
    numCorrect: Number
  }
}
</script>
