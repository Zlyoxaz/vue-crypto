<template>
	<h2>Избранное</h2>
    <div @click="getFromFavs(index)" v-for="(el, index) in uniqFavs" :key="index">
        <span>{{ el.from }}</span>
        <span> {{ el.to }}</span>
    </div>
</template>


<script>
export default {
    props: {
        uniqFavs: {
            type: Array,
            required: true
        },
        getFromFavs: {
            type: Function,
            required: true
        }
    }
}
</script>


<style scoped>
h2 {
    color: #fff;
}

div {
    display: flex;
    justify-content: space-between;
    width: 500px;
    margin: 10px auto;
    transition: all 500ms ease;
}

span {
    pad: 10px 15px;
    background: #8c00ff;
    color: #fff
}

div:hover {
    opacity: 0.7;
    transform: scale(1.1);
    cursor: pointer;
}

</style>