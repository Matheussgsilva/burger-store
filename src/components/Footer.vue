<template>
    <footer id="footer">
        <p>Make your Burger &copy; 2022</p>
    </footer>
</template>
<script>
    export default {
        name: 'Footer'
    }
</script>
<style scoped>
    #footer {
        position: fixed;
        height: 100px;
        background-color: #222;
        border-top: 4px solid #111;
        color: #FCBA03;
        display: flex;
        justify-content: center;
        align-items: center;
        bottom: 0;
        width: 100%;
    }
</style>