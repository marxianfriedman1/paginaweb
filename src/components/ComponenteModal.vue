<template>
    <div v-if="visible" class="modal">
      <div class="modal-content">
        <button class="close-button" @click="closeModal">X</button>
        <div class="body">
          <div class="image-container">
            <img :src="producto.img" :alt="producto.alt" class="modal-img" />
          </div>
          <div class="text-container">
            <h5><strong>{{ producto.title }}</strong></h5>
            <strong><em>Descripcion:</em></strong> 
            <p>{{ producto.price }}</p>
          </div>
          <a 
            :href="`https://wa.me/${telefono}?text=${mensaje}`" 
            target="_blank"
            rel="noopener noreferrer"
            class="btn btn-primary" 
            >
            Comprar
            </a>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ComponenteModal',
    props: {
      visible: {
        type: Boolean,
        required: true,
      },
      producto: {
        type: Object,
        required: null,
      },
    },
    emits: ['close'],
    methods: {
      closeModal() {
        this.$emit('close');
      },
    },
    computed: {
    telefono() {
      // Número de WhatsApp (modificar según tu necesidad, debe estar en formato internacional)
      return '573102775697'; 
    },
    mensaje() {
      // Mensaje personalizado con información del producto
      return encodeURIComponent(
        `¡Hola! Estoy interesado en el producto "${this.producto.title}" con precio ${this.producto.price}.`
      );
    },
  },
};
  </script>
  
  <style scoped>
  /* General Modal Styles */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #ffffff7a;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 2;
  padding: 10px; /* Espaciado para márgenes */
}

.modal-content {
  background: linear-gradient(45deg, rgb(255, 0, 119), rgb(255, 123, 0), rgb(255, 4, 192));
  border-radius: 10px;
  padding: 20px;
  width: 90%; /* Base para dispositivos pequeños */
  max-width: 600px; /* Ancho máximo */
  height: 70%;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  position: relative;
  z-index: 3;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.close-button {
  position: absolute;
  top: 0;
  right: 0;
  border: none;
  background: none;
  font-size: 12px;
  cursor: pointer;
}

.close-button:hover {
  background: linear-gradient(90deg, #a70e0e, #093de6);
  color: white;
  border-top-right-radius: 10px;
}

/* Body and Containers */
.body {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  width: 100%;
}

.image-container {
  width: 100%;

  background: #f5f5f5; /* Fondo en caso de imágenes con bordes transparentes */
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  overflow: hidden;
}

.modal-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: cover; /* Asegura que la imagen mantenga proporción sin deformarse */
}

.text-container {
  width: 100%;
  text-align: center;
}

.product-title {
  font-size: 1.5rem;
  font-weight: bold;
}

.product-price {
  font-size: 1.2rem;
  color: #333;
}

/* Button */
.btn {
  display: inline-block;
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  text-decoration: none;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #0056b3;
}

/* Responsive Styles */
@media (min-width: 768px) {
  .modal-content {
    width: 70%;
  }
}

@media (min-width: 992px) {
  .modal-content {
    width: 50%;
  }

  .modal-img {
    max-height: 300px; /* Ajuste en pantallas grandes */
  }
}
  </style>