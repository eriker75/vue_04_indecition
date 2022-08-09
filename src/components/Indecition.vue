<template>
    <img v-if="imagen" :src="imagen" alt="bg">
    <div class="bg-dark"></div>  
    <div class="indecision-container">
        <input v-model="question" type="text" placeholder="Hazme una pregunta">
        <p>Recuerda terminar con un signo de interrogacion (?)</p>
        <div>
            <h2>{{question}}</h2>
            <h1>{{answer}}</h1>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            question: null,
            answer: null,
            imagen: null,
            isValidQuestion: false
        }
    },
    methods:{
        async getAnswer(){
            this.answer = 'Pensando...';
            this.isValidQuestion = false;
            const {answer, image} = await fetch('https://yesno.wtf/api').then( r => r.json());
            this.isValidQuestion = true;
            this.imagen = image;
            this.answer = answer === 'yes' ? 'SI!' : 'NO!';
        }
    },
    watch: {
        question(value,oldValue){
            if(!value.includes('?')) return;
            this.getAnswer();
        }
    }
}
</script>


<style scoped>

    img, .bg-dark {
        height: 100vh;
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
    }

    .bg-dark {
        background-color: rgba(4, 38, 4, 0.3);
    }

    .indecision-container {
        position: relative;
        z-index: 99;
    }
    
    input {
        width: 250px;
        padding: 10px 15px;
        border-radius: 5px;
        border: none;
    }
    input:focus {
        outline: none;
    }

    p {
        color: white;
        font-size: 20px;
        margin-top: 0px;
    }

    h1, h2 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }

</style>
