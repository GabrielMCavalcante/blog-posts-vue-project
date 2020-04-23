<template>
    <nav>
        <ul>
            <li><router-link to="/" exact>Blog</router-link></li>
            <li><router-link to="/add" exact>Add a New Blog</router-link></li>
        </ul>
    </nav>    
</template>

<script>
export default {
    name: "Header"
}
</script>

<style scoped>
    ul{
    list-style-type: none;
    text-align: center;
    margin: 0;
    }
    li{
        display: inline-block;
        margin: 0 10px;
    }
    a{
        color: #fff;
        text-decoration: none;
        padding: 6px 8px;
        border-radius: 10px;
    }
    nav{
        background-color: rgb(61, 64, 67);;
        padding: 14px 0;
        position: fixed;
        top: 0;
        width: 100%;
        z-index: 10;
    }
    .router-link-active {
        color: rgb(208, 204, 197);
        background-color: rgb(29, 31, 32);
    }
</style>