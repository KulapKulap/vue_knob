<template>
    <ul>
        <li @click="setCrypto('BTC')" :className="cryptoNow=='BTC' ? 'active' : ''">BTC</li>
        <li @click="setCrypto('ETH')" :className="cryptoNow=='ETH' ? 'active' : ''">ETH</li>
        <li @click="setCrypto('USD')" :className="cryptoNow=='USD' ? 'active' : ''">USDT</li>
    </ul>
</template>


<script>
export default{
    props:{
        setCrypto:{
            type:Function,
            required:true
        },
        cryptoNow:{
            type:String,
            required:true
        }
    }
}
</script>

<style scoped>
/* for center - margin:0 auto */
ul{
    list-style-type: none;
    width: 200px;
    padding: 0;
    border-radius: 3px;
    margin: 0 auto;
}

li{
    display: block;
    background:red;
    color: white;
    padding: 20px 0;
    margin: 5px;
    border-radius: 5px;
}

li:hover, li:active {
    background: white;
    color: black;
    cursor: pointer;
    transition: background-color 0.3s, color 0.3s; /* Transitioning background color and text color */
}
</style>
