<template>
    <div class="time-container">
        <div v-for="(city, index) in cities" :key="index" class="city-time">
            <h2>{{ city.name }}</h2>
            <p>{{ city.localTime }}</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            cities: [
                { name: 'Zurich', offset: 1, localTime: '' },
                { name: 'London', offset: 0, localTime: '' },
                { name: 'New York', offset: -5, localTime: '' }
            ]
        };
    },
    methods: {
        getLocalTime(offset) {
            const now = new Date();
            const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
            const localTime = new Date(utc + (3600000 * offset));
            const hours = localTime.getHours().toString().padStart(2, '0');
            const minutes = localTime.getMinutes().toString().padStart(2, '0');
            const seconds = localTime.getSeconds().toString().padStart(2, '0');
            return `${hours}:${minutes}:${seconds}`;
        }
    },
    created() {
        this.updateTimes(); 
        setInterval(this.updateTimes, 1000);
    },
    methods: {
        updateTimes() {
            this.cities.forEach(city => {
                city.localTime = this.getLocalTime(city.offset);
            });
        }
    }
};
</script>


<style scoped>
.time-container {
    display: flex;
    justify-content: space-between;
}

.city-time {
    flex: 1;
    margin-right: 20px;
}
</style>
