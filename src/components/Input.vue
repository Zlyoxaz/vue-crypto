<template>
	<input min="0" type="number" @input="changeAmount($event.target.value)" placeholder="Введите число"><br>
	<button @click="convert()">Конвертировать</button>
	<button @click="favourite()" class='fav-button'>В избранное</button>	
</template>


<script>
export default {
	props: {
		changeAmount: {
			type: Function,
			required: true
		},
		convert: {
			type: Function,
			required: true
		},
		favourite: {
			type: Function,
			required: true
		}
	}
}
</script>


<style scoped>
input {
	outline: none;
	width: 500px;
	position: relative;
	top: -50px;
	font-size: 1.2em;
	border-radius: 3px;
	border: 0;
	padding: 10px 15px;
	background: #fafafa;
	color: black;
}

button {
    position: relative;
    top: -20px;
    padding: 15px 20px;
    color: #fff;
    text-transform: uppercase;
    cursor: pointer;
    background: #1a032d;
    border-radius: 3px;
	transition: transform 500ms ease;
}

button:hover {
	transform: scale(1.1);
}

.fav-button {
	margin-left: 20px;
}

</style>