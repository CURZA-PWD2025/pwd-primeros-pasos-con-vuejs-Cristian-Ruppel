<script setup>
import { ref } from 'vue'

// Variables para los inputs
const nombreUsuario = ref('')
const correo = ref('') 
const password = ref('')
const fechaNacimiento = ref('')

// Objeto usuario (se llenará al enviar el formulario)
const usuario = ref({
  nombre: '',
  email: '',
  password: '',
  fechaNacimiento: '',
  edad: 0
})

// Función para calcular edad (a partir de la fecha)
const calcularEdad = (fecha) => {
  const hoy = new Date()
  const nacimiento = new Date(fecha)
  let edad = hoy.getFullYear() - nacimiento.getFullYear()
  // Ajuste si aún no ha pasado el cumpleaños este año
  if (hoy.getMonth() < nacimiento.getMonth() || 
      (hoy.getMonth() === nacimiento.getMonth() && hoy.getDate() < nacimiento.getDate())) {
    edad--
  }
  return edad
}

// Función para guardar los datos
const guardarUsuario = () => {
  // Llenamos el objeto usuario
  usuario.value = {
    nombre: nombreUsuario.value,
    email: correo.value,
    password: password.value,
    fechaNacimiento: fechaNacimiento.value,
    edad: calcularEdad(fechaNacimiento.value)
  }
  
  // Mostramos los datos en un alert
  alert(`
    Usuario creado:
    Nombre: ${usuario.value.nombre}
    Email: ${usuario.value.email}
    Fecha Nacimiento: ${usuario.value.fechaNacimiento}
    Edad: ${usuario.value.edad} años
  `)
  
  // Limpiamos el formulario (opcional)
  nombreUsuario.value = ''
  correo.value = ''
  password.value = ''
  fechaNacimiento.value = ''
}
</script>

<template>
  <form @submit.prevent="guardarUsuario">
    <label>
      Nombre de usuario:
      <input type="text" v-model="nombreUsuario" required>
    </label>
    <label>
      Correo electrónico: 
      <input type="email" v-model="correo" required>
    </label>
    <label>
      Contraseña:
      <input type="password" v-model="password" required>
    </label>
    <label>
      Fecha de nacimiento:
      <input type="date" v-model="fechaNacimiento" required>
    </label>
    <button type="submit">Crear Usuario</button>
  </form>

  <!-- (Opcional) Mostrar datos en vivo -->
  <div v-if="usuario.nombre" class="datos-usuario">
    <h3>Datos ingresados:</h3>
    <p>Nombre: {{ usuario.nombre }}</p>
    <p>Email: {{ usuario.email }}</p>
    <p v-if="usuario.fechaNacimiento">Edad: {{ usuario.edad }} años</p>
  </div>
</template>

<style scoped>
/* Estilos básicos */
form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-width: 400px;
  margin: 0 auto;
}

label {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

button {
  padding: 0.5rem;
  background: #42b883;
  color: white;
  border: none;
  cursor: pointer;
}

.datos-usuario {
  margin-top: 2rem;
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
}
</style>