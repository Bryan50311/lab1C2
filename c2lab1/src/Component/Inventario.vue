<script setup>
import { ref } from 'vue'

// Reactive variables (>= 5 requested)
const name = ref('')
const quantity = ref('')
const price = ref('')
const category = ref('')
const products = ref([])
const errorMessage = ref('')

const categories = ['Alimentos', 'Bebidas', 'Limpieza', 'Cuidado Personal', 'Otros']

const addProduct = () => {
  // Reset error
  errorMessage.value = ''

  // Validations
  if (!name.value || !quantity.value || !price.value || !category.value) {
    errorMessage.value = 'Todos los campos son obligatorios.'
    return
  }

  if (quantity.value <= 0) {
    errorMessage.value = 'La cantidad debe ser mayor a 0.'
    return
  }

  if (price.value <= 0) {
    errorMessage.value = 'El precio debe ser mayor a 0.'
    return
  }

  // Adding product
  products.value.push({
    id: Date.now(),
    name: name.value,
    quantity: quantity.value,
    price: price.value,
    category: category.value
  })

  // Reset form
  name.value = ''
  quantity.value = ''
  price.value = ''
  category.value = ''
}
</script>

<template>
  <div class="container">
    <h1>Gestión de Inventario local</h1>
    
    <div class="card form-card">
      <form @submit.prevent="addProduct">
        <h2>Registrar Producto</h2>
        
        <p v-if="errorMessage" class="error-message">
          <span>Error:</span> {{ errorMessage }}
        </p>
        
        <div class="form-group">
          <label for="name">Nombre del Producto</label>
          <input id="name" type="text" v-model="name" placeholder="Ej. Arroz">
        </div>

        <div class="form-group">
          <label for="category">Categoría</label>
          <select id="category" v-model="category">
            <option disabled value="">Seleccione una categoría</option>
            <option v-for="(cat, index) in categories" :key="index" :value="cat">
              {{ cat }}
            </option>
          </select>
        </div>
        
        <div class="row">
          <div class="form-group">
            <label for="quantity">Cantidad</label>
            <input id="quantity" type="number" v-model.number="quantity" placeholder="Ej. 10">
          </div>
          
          <div class="form-group">
            <label for="price">Precio ($)</label>
            <input id="price" type="number" step="0.01" v-model.number="price" placeholder="Ej. 5.50">
          </div>
        </div>
        
        <!-- Usage of v-bind for dynamic classes -->
        <button type="submit" :class="{'btn-disabled': !name || !category}">Agregar al Inventario</button>
      </form>
    </div>

    <div class="card table-card">
      <h2>Productos Registrados</h2>
      
      <p v-if="products.length === 0" class="empty-msg">
        Aún no hay productos registrados. Por favor agrega uno en el formulario de arriba.
      </p>
      
      <table v-else>
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Categoría</th>
            <th>Cantidad</th>
            <th>Precio Unitario</th>
            <th>Total</th>
          </tr>
        </thead>
        <tbody>
          <!-- Dynamic row creation using v-for -->
          <tr v-for="item in products" :key="item.id">
            <td>{{ item.name }}</td>
            <td><span class="badge">{{ item.category }}</span></td>
            <td>{{ item.quantity }}</td>
            <td>Q{{ item.price.toFixed(2) }}</td>
            <td>Q{{ (item.quantity * item.price).toFixed(2) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
}

h2 {
  margin-top: 0;
  color: #34495e;
  border-bottom: 2px solid #eee;
  padding-bottom: 10px;
}

.card {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  margin-bottom: 30px;
}

.form-group {
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
}

.row {
  display: flex;
  gap: 15px;
}
.row .form-group {
  flex: 1;
}

label {
  font-weight: bold;
  margin-bottom: 5px;
  color: #555;
}

input, select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}

button {
  background-color: #4CAF50;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  width: 100%;
  margin-top: 10px;
  font-weight: bold;
}

button:hover {
  background-color: #45a049;
}

.btn-disabled {
  opacity: 0.7;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

th, td {
  border: 1px solid #ddd;
  padding: 12px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
  color: #333;
}

tr:nth-child(even) {
  background-color: #f9f9f9;
}

tr:hover {
  background-color: #f1f1f1;
}

.error-message {
  background-color: #ffebee;
  color: #c62828;
  padding: 10px;
  border-left: 4px solid #c62828;
  border-radius: 4px;
  margin-bottom: 15px;
}

.error-message span {
  font-weight: bold;
}

.empty-msg {
  text-align: center;
  color: #777;
  font-style: italic;
  padding: 20px 0;
}

.badge {
  background-color: #e0f7fa;
  color: #006064;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 0.85em;
  font-weight: bold;
}
</style>
