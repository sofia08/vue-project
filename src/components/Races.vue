<template>
    <table class="table table-bordered table-hover">
        <thead class="thead-dark">
            <tr>
                <th scope="col">Meeting Name</th>
                <th scope="col">Race Number</th>
                <th scope="col">Countdown </th>
            </tr>
        </thead>
        <tbody>
            <tr v-if='value.length == 0'>
                There are no items to display
            </tr>
            <tr v-else v-for='item in value' :key='item.race_id'>
                <td>{{ item.meeting_name }}</td>
                <td>{{ item.race_number }}</td>
                <td>{{ formatCountdown(item.advertised_start.seconds) }}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    name: 'Races',
    props: {
        value: Array
    },
    data () {
        return {
            time: this.getCurrentTime()
        }
    },
    created() {
        // Update the timer every second
        window.setInterval(() => this.time = this.getCurrentTime(), 1000)
    },
    methods: {
        getCurrentTime() {
            return Math.floor(Date.now() / 1000)
        },
        formatCountdown(start_time) {
            let time_left = start_time - this.time

            // The event is occurring
            if (time_left <= 0) {
                if (time_left <= -60) {
                    console.log("Remove element from the list")
                }
                return 'now'
            } else if (time_left / 60 >= 1) {
                return `${Math.ceil(time_left / 60)}m`
            }

            return `${start_time - this.time}s`
        }
    }
}
</script>


<style scoped>

</style>