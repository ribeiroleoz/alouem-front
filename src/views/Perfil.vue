<template>
  <div class="main">
      <ul class="nav justify-content-end nav-bar ">
          <li class="nav-item ">
              <div class="">
                  <a role="button"
                  class="nav-link active me-4 link-perfil" 
                  aria-current="page" 
                  @click="irHome()">
                      <i class="bi bi-house"></i> Home
                  </a>
              </div>
          </li>
      </ul>
      <div class="d-flex">      
        <div class="card row col-md-12 col-10 ">
              <div class="perfil row">

                <div class="col-2">
                  <i class="bi bi-person-circle"></i>
                </div>

                <div class="dados col">

                  <div class="col-md-10 col-6">
                    <label for="title" class="font-text" 
                    style="color: black; font-family: 'Red Hat Display', sans-serif;">
                    Nome de Usuário:
                    </label>
                    <input  
                    disabled
                    :value="nome_usuario"  
                    type="text" 
                    class="form-control w-65">
                  </div>

                  <div class="col-md-10 col-6">
                    <label for="title" class="font-text" 
                    style="color: black; font-family: 'Red Hat Display', sans-serif;">
                    Email educacional:
                    </label>
                    <input
                    disabled  
                    v-model="email" 
                    type="text" 
                    class="form-control w-65">
                  </div>
                </div>     
              </div>

              <div class="row">

                <div class="publi" style="">

                  <label for="title" class="font-text" 
                  style="color: black; font-family: 'Red Hat Display', sans-serif; background-color: white; width: 100%;">
                  Publicações feitas:
                  </label>

                  <div class="d-flex flex-column mb-4">
                    <CardPublicacaoPerfil
                        v-for="(publicacao, index) in publicacoes"
                        :key="index"
                        :publicacao="publicacao"
                        />
                  </div>


                </div>

                <div class="coment">

                  <label for="title" class="font-text" 
                  style="color: black; font-family: 'Red Hat Display', sans-serif; background-color: white; width: 100%;">
                  Comentarios feitos:
                  </label>

                  <div class="d-flex flex-column mb-4">
                    <CardComentario
                        :id-usuario="id"
                        />
                  </div>
                  
                </div>
              </div>

            </div>
      </div>
  </div>
</template>
<script setup>
import CardPublicacaoPerfil from '../components/CardPublicacaoperfil.vue';
import CardComentario from '../components/CardComentario.vue';
import { ref, onBeforeMount } from 'vue';
import http from '../services/http.js'
import { useRouter } from 'vue-router';

const route = useRouter();

const publicacoes = ref([]);
const comentarios = ref([]);
const userLogado = ref(null);
const nome_usuario = ref(null);
const email = ref(null);
const id = ref(null);

function irHome(){
    route.push({name: 'home'})
}

onBeforeMount(() => {
  carregarUsuarioLogado();
  carregarPublicacoes();
});

function carregarUsuarioLogado() {
  const jsonString = localStorage.getItem('usuario');
  const objeto = JSON.parse(jsonString);
  userLogado.value = objeto;
  nome_usuario.value = userLogado.value.Nome;
  email.value = userLogado.value.Email;
  id.value = userLogado.value.IdUsuario
}


async function carregarPublicacoes() {
    await http.get(`publicacao/usuario/${id.value}`)
        .then(response => {
            publicacoes.value = response.data
        })
        .catch(error => {
            console.error("Erro ao carregar publicações: " + error);
        })
    
}


</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins&family=Red+Hat+Display:wght@600&display=swap');
@import "bootstrap-icons/font/bootstrap-icons.css";
.main {
  background-color: #003049;
  background-size: cover;
  margin: 0px;
}
.card {
  background-color: #003049;
  border-radius: 0px 0px 5px 5px;
  height: 96vh;
}
.nav-bar {
  background-color: #072231;
  height: 4vh;
}
.bi-person-circle{
  font-size: 80px;
}

.link-perfil{
  display: flex;
  color: bisque;

}

.perfil.row{
  background-color: #F8E2CF;
  border-radius: 0px 0px 5px 5px;
  max-width: 650px;
  max-height: 550px;
  padding: 24px;
  margin-top: 44px;
  margin-left: 44px;
}

.publi{
  overflow-y: scroll;
  background-color: #F8E2CF;
  border-radius: 0px 0px 5px 5px;
  width: 650px;
  height: 440px;
  margin-top: 44px;
  margin-left: 44px;
}
.file-imagens{
    display: flex;
    position: relative;
    border: 2px none #e2e2e2;
    transition: 0.3s;
    margin-top: 24px;
    padding: 0;
    border-radius: 5px;
    background-color: var(--bs-body-bg);
    width: 486px;
    min-height: 300px;
    justify-content: center;
    
}
.coment{
  overflow-y: scroll;
  background-color: #F8E2CF;
  border-radius: 0px 0px 5px 5px;
  width: 650px;
  height: 440px;
  margin-top: 44px;
  margin-left: 44px;
}
.dados {
  margin-left: 44px;
}

.create-pub{
  background-color: #303030;
  border-radius: 0px 0px 5px 5px;
  margin-top: -0.1rem !important;
  width: 95%;
  margin-left: 2.5%;
}

.file-imagens{
  display: flex;
  position: relative;
  border: 2px none #e2e2e2;
  transition: 0.3s;
  margin-top: 24px;
  padding: 0;
  border-radius: 5px;
  background-color: var(--bs-body-bg);
  width: 486px;
  min-height: 300px;
  justify-content: center;
  
}

.btn-custom {
background-color: #B0B0B0; 
color: #000000; 
border-radius: 15px;
font-family: 'Red Hat Display', sans-serif;
}

.btn-custom:hover {
background-color: #303030; 
color: #ffffff; 
border-radius: 15px;
border: 1px none #e2e2e2;
}

.text-file{
  margin-top: 4px;
  font-size: 13px;
  font-family: 'Red Hat Display', sans-serif;
  color: #B0B0B0;
}

</style>