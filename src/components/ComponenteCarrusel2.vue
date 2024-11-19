<template>
    <div class="main-container">
      <div class="container my-5">
        <h2>Ropa</h2>
        <div ref="productCarousel" id="productCarousel" class="carousel slide">
          <!-- Controles para navegar -->
          <button
            class="carousel-control-prev custom-control-prev"
            type="button"
            @click="prevSlide"
          >
            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
          </button>
          <button
            class="carousel-control-next custom-control-next"
            type="button"
            @click="nextSlide"
          >
            <span class="carousel-control-next-icon" aria-hidden="true"></span>
          </button>
  
          <div class="carousel-inner">
            <!-- Grupos dinámicos -->
            <div
              v-for="(grupo, index) in grupos"
              :key="index"
              :class="['carousel-item', { active: index === 0 }]"
            >
              <div class="row">
                <div
                  class="col-12 col-sm-6 col-md-4 col-lg-3"
                  v-for="(producto, index) in grupo"
                  :key="index"
                >
                  <div class="card">
                    <img
                      :src="producto.img"
                      class="card-img-top"
                      :alt="producto.alt"
                    />
                    <div class="card-body">
                      <h5 class="card-title">{{ producto.title }}</h5>
                      <p class="card-text">{{ producto.price }}</p>
                      <a href="#" class="btn btn-primary" @click="openModal(producto)">
                        Ver Producto
                      </a>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <ComponenteModal
        :visible="isModalVisible"
        :producto="selectedProducto"
        @close="closeModal"
      />
    </div>
  </template>
  
  <script>
  import { onMounted, onUnmounted, ref } from "vue";
  import Carousel from "bootstrap/js/dist/carousel";
  import ComponenteModal from "./ComponenteModal.vue";
  
  import LoroImg from "@/assets/Loro.jpg";
  import gettyimage from "@/assets/gettyimage.jpg";
  import camiseta from "@/assets/camiseta.jpg";
  import chaqueta from "@/assets/chaqueta.jpg";
  import chaqueta2 from "@/assets/chaqueta2.jpg";
  import collar from "@/assets/collar.jpg";
  import collar2 from "@/assets/collar2.jpg";
  import gorreon from "@/assets/gorreon.jpg";
  
  export default {
    name: "ComponenteCarrusel2",
    components: {
      ComponenteModal,
    },
    setup() {
      const productCarousel = ref(null);
      const isModalVisible = ref(false);
      const selectedProducto = ref(null);
      const grupos = ref([]);
  
      const productos = [
        { img: LoroImg, alt: "Producto 1", title: "Producto 1", price: "$100.00" },
        { img: gettyimage, alt: "Producto 2", title: "Producto 2", price: "$120.00" },
        { img: camiseta, alt: "Producto 3", title: "Producto 3", price: "$150.00" },
        { img: chaqueta, alt: "Producto 4", title: "Producto 4", price: "$200.00" },
        { img: chaqueta2, alt: "Producto 5", title: "Producto 5", price: "$110.00" },
        { img: collar, alt: "Producto 6", title: "Producto 6", price: "$130.00" },
        { img: collar2, alt: "Producto 7", title: "Producto 7", price: "$170.00" },
        { img: gorreon, alt: "Producto 8", title: "Producto 8", price: "$210.00" },
      ];
  
      let carouselInstance = null;
  
      // Configurar el carrusel al montar el componente
      onMounted(() => {
        if (productCarousel.value) {
          carouselInstance = new Carousel(productCarousel.value, {
            interval: false,
          });
        }
        calcularGrupos(); // Cálculo inicial
        window.addEventListener("resize", calcularGrupos);
      });
  
      // Eliminar listeners al desmontar
      onUnmounted(() => {
        window.removeEventListener("resize", calcularGrupos);
      });
  
      const prevSlide = () => carouselInstance && carouselInstance.prev();
      const nextSlide = () => carouselInstance && carouselInstance.next();
  
      const openModal = (producto) => {
        selectedProducto.value = producto;
        isModalVisible.value = true;
      };
  
      const closeModal = () => {
        isModalVisible.value = false;
      };
  
      const calcularGrupos = () => {
        const width = window.innerWidth;
        let itemsPerGroup;
  
        if (width >= 992) {
          itemsPerGroup = 4;
        } else if (width >= 768) {
          itemsPerGroup = 3;
        } else if (width >= 576) {
          itemsPerGroup = 2;
        } else {
          itemsPerGroup = 1;
        }
  
        grupos.value = [];
        for (let i = 0; i < productos.length; i += itemsPerGroup) {
          grupos.value.push(productos.slice(i, i + itemsPerGroup));
        }
  
        // Controlar visibilidad de los botones
        const hasMultipleSlides = grupos.value.length > 1;
        const prevButton = document.querySelector(".custom-control-prev");
        const nextButton = document.querySelector(".custom-control-next");
  
        if (prevButton && nextButton) {
          prevButton.classList.toggle("hidden", !hasMultipleSlides);
          nextButton.classList.toggle("hidden", !hasMultipleSlides);
        }
      };
  
      return {
        productCarousel,
        prevSlide,
        nextSlide,
        isModalVisible,
        selectedProducto,
        openModal,
        closeModal,
        grupos,
      };
    },
  };
  </script>
  
  <style scoped>
  h2 {
    background: linear-gradient(45deg, red, blue, green);
    color: white;
    border-radius: 10px;
    padding: 10px;
    text-align: center;
  }
  
  .card-img-top {
    object-fit: cover;
    width: 100%;
    aspect-ratio: 4/5;
  }
  
  .custom-control-prev,
  .custom-control-next {
    width: 50px;
    height: 50px;
    top: 50%;
    transform: translateY(-50%);
    background-color: rgba(0, 0, 0, 0.7);
    border-radius: 50%;
  }
  
  .custom-control-prev {
    left: -30px;
  }
  
  .custom-control-next {
    right: -30px;
  }
  
  .custom-control-prev.hidden,
  .custom-control-next.hidden {
    visibility: hidden;
  }
  </style>
  