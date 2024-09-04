<template>
    <h1>แสดงข้อมูลนักศึกษา {{ username }}</h1>
</template>
<script>
 export default{
    data: () => ({
        id : '',
        username: '',
        password: '',
        status: '',
        email: '',
        picture: '',
    }),
    async created (){
        const response = await fetch('http://localhost:7000/list')
        console.log(response);
        const data = await response.json()
        console.log('data=>' ,data);
        this.username = data.row[1].username
    }
 }
</script>