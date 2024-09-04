<template>
    <div>
        <h1>แสดงข้อมูลนักศึกษา</h1>
        <ul>
            <li v-for="st in students" :key="st.code">
                <span>{{ st.code }}</span>
                <span>{{ st.username }}</span>
                <span>{{ st.password }}</span>
            </li>
        </ul>
    </div>
</template>
<script>
import axios from "axios";
    export default{
        data: () => ({
            students:[
                {id: '12345', username: 'tawan', password: ' 54321'},
                {id: '12345', username: 'tawan', password: ' 54321'},
                {id: '12345', username: 'tawan', password: ' 54321'},
                {id: '12345', username: 'tawan', password: ' 54321'},
            ]
        }),
        async created (){
        const response = await axios.get('http://localhost:7000/list')
        const data = response.data
        this.students = data.row
        console.log(data);
    }
    }
</script>