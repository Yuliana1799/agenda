<template>
  <div class="contenedor">
    <div class="bloque1">
      <input type="text" id="tarea" name="tarea" v-model="actividad" placeholder="Ingrese en nombre de su Tarea"><br>
      <input type="date" id="fecha_tarea" name="fecha_tarea" v-model="fecha_tarea">
      <input type="checkbox" name="priorizar" v-model="priorizar">
    </div>
    <div class="bloque2">
      <button @click="agregar()" id="agregar">Agregar</button>
      <button @click="ordenar()" id="ordenar">Ordenar</button>
    </div>
    <div class="bloque3">
      <table>
        <thead>
          <tr>
            <th>Actividad</th>
            <th>Prioridad</th>
            <th>Fecha</th>
            <th>Opciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, i) in registros" :key="i"
            :style="item.priorizar === 'Alta' ? 'background-color: red; color: white;' : ''">

            <td>{{ item.actividad }}</td>
            <td>{{ item.priorizar }}</td>
            <td>{{ item.fecha_tarea }}</td>
            <td>
              <button @click="eliminar(i)">❌</button>
              <button @click="editar(item, i)">📝</button>
            </td>
          </tr>

        </tbody>

      </table>
    </div>
    <div v-if="mostrarAlerta" class="alerta">
        <p>{{ mostrarAlerta }}</p>
        <button @click="cerrarAlerta">Cerrar</button>
      </div>
  </div>
</template>


<script setup>
import { ref } from "vue"
const registros = ref([])


const actividad = ref("")
const priorizar = ref("")
const fecha_tarea = ref("")

const mostrarAlerta = ref("");

function agregar() {
  if (actividad.value === "") {
    mostrarAlerta.value = "Ingrese una tarea";
  } else if (fecha_tarea.value === "") {
    mostrarAlerta.value = "Ingrese una fecha";
  } else if (new Date(fecha_tarea.value) < new Date()) {
    mostrarAlerta.value = "La fecha no puede ser menor a la actual";
  } else {
    mostrarAlerta.value = "¡Registro exitoso!";

    registros.value.push({
      actividad: actividad.value,
      priorizar: priorizar.value ? "Alta" : "Baja",
      fecha_tarea: fecha_tarea.value,
    });
    ({
      actividad: (actividad.value = ""),
      priorizar: (priorizar.value = ""),
      fecha_tarea: (fecha_tarea.value = ""),
    });
  }
} 

function cerrarAlerta() {
  mostrarAlerta.value = false;


}

function eliminar(i) {
  registros.value.splice(i, 1)
}

function ordenar() {
  registros.value.sort((a, b) => {
    if (a.priorizar === 'Alta' && b.priorizar !== 'Alta') {
      return -1;
    }
    else if (b.priorizar === 'Alta' && a.priorizar !== 'Alta') {
      return 1;
    }
    else {
      return 0;
    }
  });

}

</script>



<style>
* {
  margin: 0;
  padding: 0;
}

body {
  background-color: rgb(204, 175, 175);

}

.contenedor {
  background-color: rgb(204, 175, 175);
  height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
}

.bloque1 {
  margin: 20px;
  display: flex;
  justify-content: center;
  margin-top: 50px;
}

#tarea {
  font-size: 25px;
  width: 550px;
  border-radius: 6px;
}

#fecha_tarea {
  font-size: 25px;
  border-radius: 6px;
}

.bloque2 {
  margin: 20px;
  text-align: right;
  margin-right: 50px;
}

#agregar {
  font-size: 25px;
  margin: 2px;
  width: 10%;
  border-radius: 6px;
  background: linear-gradient(to right, #d1ebca, #b7f195, #44f744);
}

input[type="checkbox"] {
  width: 25px;
  height: 25px;
  margin: 2px;
}

#ordenar {
  font-size: 25px;
  margin: 2px;
  width: 10%;
  border-radius: 6px;
  background: linear-gradient(to right, #ebcae4, #ee95f1, #f144f7);
}

table {
  text-align: center;
  width: 96%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  padding: 10px;
  text-align: left;
  border: 1px solid #ddd;
  text-align: center;
}

th {
  background-color: #f2f2f2;
  font-size: 25px;
}

.bloque3 {
  display: flex;
  justify-content: center;

}
.alerta {
  background-color: #f8d7da;
  color: #721c24;
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 15%;
  height: 15vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  border: 1px solid rgb(223, 43, 139);
}

button {
  margin-top: 15px;
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 8px 10px;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  background-color: #1a1a1a;
  cursor: pointer;
  transition: border-color 0.25s;
  color: #ffffff;
}
button:hover {
  border-color: #646cff;
}

</style>







