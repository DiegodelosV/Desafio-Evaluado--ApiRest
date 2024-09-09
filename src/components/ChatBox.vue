<template>
  <div class="app-chat">
    <!-- Usuario 1 -->
    <div class="usuario-contenedor" v-if="usuarios.length">
      <img :src="usuarios[0].picture.large" class="avatar" />
      <p class="nombre">
        {{ usuarios[0].name.first }} {{ usuarios[0].name.last }}
      </p>
      <input type="color" v-model="colorUsuario1" />
      <textarea v-model="mensajeUsuario1" placeholder="Escribe tu mensaje" />
      <button @click="enviarMensaje(1)">Enviar</button>
    </div>

    <!-- Chatbox -->
    <div class="chat-contenedor">
      <div
        v-for="(mensaje, index) in mensajes"
        :key="index"
        :class="mensaje.emisor === 1 ? 'mensaje-izquierda' : 'mensaje-derecha'"
        :style="{
          backgroundColor: mensaje.emisor === 1 ? colorUsuario1 : colorUsuario2,
        }"
        class="mensaje"
      >
        <p>
          <strong>
            {{
              mensaje.emisor === 1
                ? usuarios[0].name.first + " " + usuarios[0].name.last
                : usuarios[1].name.first + " " + usuarios[1].name.last
            }}
            :
          </strong>
          {{ mensaje.texto }}
        </p>
      </div>
    </div>

    <!-- Usuario 2 -->
    <div class="usuario-contenedor" v-if="usuarios.length">
      <img :src="usuarios[1].picture.large" class="avatar" />
      <p class="nombre">
        {{ usuarios[1].name.first }} {{ usuarios[1].name.last }}
      </p>
      <input type="color" v-model="colorUsuario2" />
      <textarea v-model="mensajeUsuario2" placeholder="Escribe tu mensaje" />
      <button @click="enviarMensaje(2)">Enviar</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      usuarios: [],
      mensajes: [],
      mensajeUsuario1: "",
      mensajeUsuario2: "",
      colorUsuario1: "#03fcf8",//color por defecto 
      colorUsuario2: "#e6608d",
    };
  },
  async mounted() {
    const { data } = await axios.get("https://randomuser.me/api/?results=2");
    this.usuarios = data.results;
  },
  methods: {
    enviarMensaje(emisor) {
      const texto = emisor === 1 ? this.mensajeUsuario1 : this.mensajeUsuario2;
      if (texto.trim()) {
        this.mensajes.push({ emisor, texto });
        emisor === 1
          ? (this.mensajeUsuario1 = "")
          : (this.mensajeUsuario2 = "");
      }
    },
  },
};
</script>
