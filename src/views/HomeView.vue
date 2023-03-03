<template>
  <div class="home" >
   <div class="flex w-full py-[10px] bg-sky-200 justify-center relative">
     <h1 class="text-center text-[#5b5b5b] font-bold text-4xl ">Usuarios registrados</h1>
     <button class="border-blue-400 w-[160px] bg-blue-500 rounded-xl py-[6px] px-[4px]  absolute right-[20px] text-white" @click="formRoute">agregar Usuario</button>
   </div> 
   
    <table class="w-full px-[5%] border-collapse border mt-10 rounded-2xl" v-if="users.length > 0">
      <tr class="font-bold border bg-[#bfcdfa] ">
        <td>Usuario</td>
        <td>nombre</td>
        <td>estado</td>
        <td>observaciones</td>
        <td>contraseña</td>
        <td>codigo nivel</td>
        <td>nivel</td>
        <td colspan="2">accion</td>
       

      </tr>
      <tr class="border hover:bg-[#bdbdbd] " v-for="user in users" :key="user.id">
        <td class="font-bold" >{{ user?.id }}</td>
        <td>{{ user?.name }}</td>
        <td> {{ user?.status ? 'activo' : 'inactivo' }} </td>
        <td>{{ user?.observation }}</td>
        <td>{{ user?.password }}</td>
        <td>{{ user?.nivel_id }}</td>
        <td>{{ user?.nivel?.name }}</td>
        <td><button class="border-blue-400 w-auto bg-blue-500 rounded-xl py-[4px] px-[6px]  text-white" @click="editUserRoute(user?.id)">Editar</button></td>
        <td><button class="border-blue-400 w-auto bg-blue-500 rounded-xl py-[4px] px-[6px]  text-white" @click="onDeleteUser(user?.id)">Eliminar</button></td>

      </tr>
    </table>
    <h2 class="text-center text-[#5b5b5b] font-bold text-4xl " v-else>cargando...</h2>


    <H1 class="text-center text-[#5b5b5b] font-bold text-4xl mt-5 mb-2">Niveles</H1>
    <form @submit.prevent="onAddNivel" class="flex px-[5%] mb-2">
      <label class="flex items-center mr-4 "> <p class="mr-5 font-bold">Agregar nivel</p>
        <input class="border rounded-lg" type="text" v-model="nivelName" required minlength="2" >
      </label>
      <button class="border-blue-400 w-auto bg-blue-500 rounded-xl py-[4px] px-[6px]  text-white" type="submit">agregar</button>
    </form>
    <ul>
      <ol class="hover:bg-[#bdbdbd] w-min flex rounded-lg px-[2px] ml-[5%] gap-10 bg-[#fff] py-[2px]" v-for="nivel in niveles" :key="nivel?.id">
        <p class="w-[200px] text-left italic truncate"><strong>{{ nivel?.id }}.</strong> {{ nivel?.name }}</p> 
        <button class="border-blue-400 w-auto bg-blue-500 rounded-xl py-[4px] px-[6px]  text-white" @click="editNivelRoute(nivel?.id)">editar</button>
        <button class="border-blue-400 w-auto bg-blue-500 rounded-xl py-[4px] px-[6px]  text-white" @click="onDeleteNivel(nivel?.id)">eliminar</button>
      </ol>
    </ul>


  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'

export default {
  name: 'HomeView',


  data() {
    return {
      users: [],
      niveles: [],
      nivelName:''    }
  },

  methods: {

    async fetchUsers() {
      const { data } = await axios.get('http://localhost:8000/api/afiliados/usuarios')
      this.users = data
    },
    editUserRoute(id) {
      // router push with path
      this.$router.push(`/formulario/${id}`)
    },
    async onDeleteUser(id) {
      await axios.delete(`http://localhost:8000/api/afiliados/usuario/${id}`)
      this.fetchUsers()
    },
    async fetchNiveles() {
      const { data } = await axios.get('http://localhost:8000/api/afiliados/niveles')
      this.niveles = data
    },
    async onDeleteNivel(id) {
      await axios.delete(`http://localhost:8000/api/afiliados/nivel/${id}`)
      this.fetchNiveles()
    },
    editNivelRoute(id) {
      // router push with path
      this.$router.push( '/nivel/' + id)
    },
    formRoute() {
      // router push with path
      this.$router.push('/formulario')
    },
    async onAddNivel() {
      await axios.post('http://localhost:8000/api/afiliados/nivel',{
        name: this.nivelName
      })
      this.fetchNiveles()
    },

  },

  mounted() {
    this.fetchUsers();
    this.fetchNiveles();
  }
}
</script>
