<template>
    <section class="section">
        <div id="clock">
            <p class="date">{{ time }}</p>
            <p class="time" :key="updateKey">{{ time }}</p>
            <p class="text">DIGITAL CLOCK with Vue.js</p>
        </div>
    </section>
</template>
  
<script>
import Card from '~/components/Card'

export default {
    name: 'IndexPage',
    components: {
        Card
    },
    data() {
        return {
            week: ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'],
            updateKey: Math.random()
        }
    },
    props: {
        time: {
            type: String,
            required: true,
        },
    },
    computed: {

    },
    methods: {

    },
    mounted() {

    }
}
</script>
  
<style lang="scss">
html,
body {
    height: 100%;
}

body {
    background: #0f3854;
    background: radial-gradient(ellipse at center, #0a2e38 0%, #000000 70%);
    background-size: 100%;
}

p {
    margin: 0;
    padding: 0;
}

#clock {
    font-family: 'Share Tech Mono', monospace;
    color: #ffffff;
    text-align: center;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    color: #daf6ff;
    text-shadow: 0 0 20px rgba(10, 175, 230, 1), 0 0 20px rgba(10, 175, 230, 0);

    .time {
        letter-spacing: 0.05em;
        font-size: 80px;
        padding: 5px 0;
    }

    .date {
        letter-spacing: 0.1em;
        font-size: 24px;
    }

    .text {
        letter-spacing: 0.1em;
        font-size: 12px;
        padding: 20px 0 0;
    }
}
</style>
  