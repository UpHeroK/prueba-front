<template>
  <div>
    <div class="flex w-full py-[10px] bg-sky-200 justify-center">
      <h1 class="text-center text-[#5b5b5b] font-bold text-4xl"> Editar Nivel {{ $route.params.id }}</h1>
      <button class="border-blue-400 w-[160px] bg-blue-500 rounded-xl py-[6px] px-[4px]  absolute left-[20px] text-white"
        @click="homeRoute">regresar</button>
    </div>
    <!-- //route id  -->
    <div class="w-full flex justify-center pt-[20px]">

      <form @submit.prevente="onSubmit" class="w-[30%] flex flex-col gap-5">
        
        <label class="w-full flex flex-col"> Nombre nivel
          <input class="border border-[#5d5d5d] px-[5px]  rounded-lg py-[5px]" type="text" v-model="name" required name="name" minlength="2">
        </label>
        <button class="border-blue-400 w-[160px] bg-blue-500 rounded-xl py-[6px] px-[4px] mx-auto block  text-white" type="submit">enviar</button>
      </form>
    </div>




  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'EditNivel',

  data() {
    return {
      name: '',
    }
  },
  methods: {

    async fetchNivel() {
      const { data } = await axios.get(`http://localhost:8000/api/afiliados/nivel/${this.$route.params.id}`)
      this.name = data.nivel.name
      console.log(data)
    },
    async onSubmit() {
      try {
        console.log(this.name)
        await axios.put(
          'http://127.0.0.1:8000/api/afiliados/nivel/' + this.$route.params.id,
          {
            name: this.name
          })
          this.$router.push('/')
      } catch (error) {

      }
    },
    homeRoute() {
      // router push with path
      this.$router.push('/')
    }
  },
  mounted() {
    this.fetchNivel();
  }

}
</script>