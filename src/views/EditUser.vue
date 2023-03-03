<template>
  <div>
    <div class="flex w-full py-[10px] bg-sky-200 justify-center">
      <h1  class="text-center text-[#5b5b5b] font-bold text-4xl"> Editar usuario {{ $route.params.id }}</h1> 
      <button class="border-blue-400 w-[160px] bg-blue-500 rounded-xl py-[6px] px-[4px]  absolute left-[20px] text-white" @click="homeRoute">regresar</button>
    </div>
   <div class="w-full flex justify-center pt-[20px]">

    <form @submit.prevent="onSubmit" class="w-[30%] flex flex-col gap-5">
      <label class="w-full flex flex-col"> Nombre de usuario
        <input class="border border-[#5d5d5d] rounded-lg px-[5px] py-[5px]" type="text" v-model="user.name" required
          minlength="2">
      </label>
      <label class="w-full flex flex-col"> Estado de usuario
        <select class="border border-[#5d5d5d]  px-[5px] rounded-lg py-[5px]" name="estado" v-model="user.status"
          required>
          <option value="1">Activo</option>
          <option value="0">Inactivo</option>
        </select>
      </label>
      <label class="w-full flex flex-col">Observaciones de usuario
        <input class="border border-[#5d5d5d] px-[5px]  rounded-lg py-[5px]" type="text" v-model="user.observation"
          required>
      </label>
      <label class="w-full flex flex-col">Password
        <input class="border border-[#5d5d5d] px-[5px]  rounded-lg py-[5px]" type="text" v-model="user.password" required>
      </label>
      <label class="w-full flex flex-col">Nivel
        <select class="border border-[#5d5d5d] px-[5px]  rounded-lg py-[5px]" name="nivel " v-model="user.nivel_id"
          required>
          <option v-for="nivel in niveles" :value="nivel.id">{{ nivel.name }}</option>
        </select>
      </label>
      <button class="border-blue-400 w-[160px] bg-blue-500 rounded-xl py-[6px] px-[4px] block mx-auto  text-white"
        type="submit">enviar</button>


    </form>
   </div>

  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'EditUser',

  data() {
    return {
      user: {},
      niveles: {}
    }
  },
  methods: {
    async fetchUser() {
      const { data } = await axios.get(`http://localhost:8000/api/afiliados/usuario/${this.$route.params.id}`)
      this.user = data.usuario
      console.log(data)
    },
    async fetchNiveles() {
      const { data } = await axios.get(`http://localhost:8000/api/afiliados/niveles`)
      this.niveles = data
    },
    async onSubmit() {
      try {
        console.log(this.user)
        await axios.put('http://localhost:8000/api/afiliados/usuario/' + this.$route.params.id, {
          name: this.user.name,
          status: this.user.status,
          observation: this.user.observation,
          password: this.user.password,
          nivel_id: this.user.nivel_id
        })
        this.homeRoute()
      }
      catch (error) {
        console.log(error)
      }
    },
    homeRoute() {
      // router push with path
      this.$router.push('/')
    }
  },

  mounted() {
    this.fetchNiveles();
    this.fetchUser();
  }

}
</script>


<style>
label {
  display: block;
}
</style>