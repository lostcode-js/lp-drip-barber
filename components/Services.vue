<template>
  <section
    id="services_section"
    class="w-full bg-primary-color pt-4 md:pt-0 relative overflow-hidden z-[5]"
  >    
    <div
      id="testimonial-container"
      class="flex items-center justify-center z-5 relative w-full"
    >   
      <Swiper
        :modules="[SwiperAutoplay, SwiperNavigation]"
        :slides-per-view="3"
        :loop="true"
        :autoplay="{
        delay: 3000,
        disableOnInteraction: false,
      }"      
      >
        <SwiperSlide
          v-for="(img, index) in imgCard"
          :key="index"          
        >
          <div
            class="image-zoom-container card text-center text-white grayscale hover:grayscale-0 select-none   mt-24 z-0 hover:z-50 overflow-hidden hover:overflow-y-hidden hidden md:flex"
          >
            <NuxtImg
              format="webp"
              :src=img.src
              :alt=img.alt
              class="image-zoom"
              fit="cover"
              height="700"
              width="1100"            
              loading="lazy"
            />       
            <div class="absolute group inset-0 flex flex-col items-center justify-center">
              <h2 class="font-bold md:text-2xl lg:text-5xl z-10" style="text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); ">{{ img.name }}</h2>            
            </div>         

          </div>

        </SwiperSlide>
      </Swiper>   
                    
      
      <!--Mobile slides-->
      <div class="sm:hidden w-full mt-10 pt-6">
        <Swiper
          :modules="[SwiperAutoplay, SwiperNavigation]"
          :slides-per-view="1"
          :loop="true"
          :autoplay="{
            delay: 3000,
            disableOnInteraction: false,
          }"
        >
          <SwiperSlide 
            v-for="(img, index) in imgCard"
            :key="index"
            >
            <div
              class="card text-center text-white mr-4 w-full flex flex-col items-center h-auto grayscale hover:grayscale-0 overflow-hidden hover:scale-110 transition duration-500 sm:hidden"              
            >             
              <NuxtImg
              format="webp"
              :src=img.src
              alt="corte de cabelo"
              class=""
              fit="cover"
              height="700"
              width="1100"            
              loading="lazy"
              />       
              <div class="absolute group inset-0  flex flex-col items-center justify-center">
                <h2 class="font-bold text-2xl z-10" style="text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); ">{{ img.name }}</h2>                
              </div>                            
            </div>
          </SwiperSlide>
          
        </Swiper>
      </div>
    </div>
  </section>
</template> 

<script setup lang="ts">
interface imgCard {
  name: string;
  link: string;
  src: string;
  alt: string;
}

const imgCard: imgCard[] = [
  { name: "BARBA ALINHADA", link: "#", src: "assets/service-1.jpg", alt: "serviços de corte e barba" },
  { name: "CAVANHAQUE", link: "#", src: "assets/service-2.jpg", alt: "clientes na barbearia" },
  { name: "JEITINHO NA SOBRANCELHA", link: "#", src: "assets/service-3.jpg", alt: "clientes na barbearia" },
  { name: "LIMPEZA DE PELE", link: "#", src: "assets/service-4.jpg", alt: "clientes na barbearia" },
  { name: "LUZES", link: "#", src: "assets/service-5.jpg", alt: "clientes na barbearia" },
  { name: "NEVADO", link: "#", src: "assets/service-6.jpg", alt: "clientes na barbearia" },
  { name: "REGUA", link: "#", src: "assets/service-7.jpg", alt: "clientes na barbearia" },
];


</script>

<style scoped>
.image-zoom-container {
  position: relative;
  overflow: hidden;
  
}

.image-zoom-container:hover .image-zoom {
  transform: scale(1.1);
  transition: transform 0.5s;
}

.image-zoom {
  transition: transform 0.5s;
  max-height: 400px;
  object-fit: cover;  
}


</style>