<template>
  <section
    id="gallery_section"
    class="w-full pb-8 md:pb-12 lg:pb-16 pt-10 md:pt-16 lg:pt-24 md:px-4 h-full relative overflow-hidden md:flex md:items-center md:justify-center"
  >
    <div class="container hidden md:flex mx-auto px-5 lg:px-44">
      <div class="-m-1 flex flex-wrap md:-m-2">
        <div class="flex w-1/2 flex-wrap">
          <div class="w-1/2 p-1 md:p-2">
            <NuxtImg
              format="webp"
              alt="gallery"
              class="block h-full w-full rounded-lg object-cover object-center"
              src="assets/gallery-item-2.jpg"
              loading="lazy"
            />
          </div>
          <div class="w-1/2 p-1 md:p-2">
            <NuxtImg
              format="webp"
              alt="gallery"
              class="block h-full w-full rounded-lg object-cover object-center"
              src="assets/gallery-item-3.jpg"
              loading="lazy"
            />
          </div>
          <div class="w-full p-1 md:p-2">
            <NuxtImg
              format="webp"
              alt="gallery"
              class="block h-full w-full rounded-lg object-cover object-center"
              src="assets/gallery-item-1.jpg"
              loading="lazy"
            />
          </div>
        </div>
        <div class="flex w-1/2 flex-wrap">
          <div class="w-full p-1 md:p-2">
            <NuxtImg
              format="webp"
              alt="gallery"
              class="block h-full w-full rounded-lg object-cover object-center"
              src="assets/gallery-item-6.jpg"
              loading="lazy"
            />
          </div>
          <div class="w-1/2 p-1 md:p-2">
            <NuxtImg
              format="webp"
              alt="gallery"
              class="block h-full w-full rounded-lg object-cover object-center"
              src="assets/gallery-item-5.jpg"
              loading="lazy"
            />
          </div>
          <div class="w-1/2 p-1 md:p-2">
            <NuxtImg
              format="webp"
              alt="gallery"
              class="block h-full w-full rounded-lg object-cover object-center"
              src="assets/gallery-item-4.jpg"
              loading="lazy"
            />
          </div>
        </div>
      </div>
    </div>

    <!--Mobile slides-->
    <div class="md:hidden px-6 sm:px-20">
      <Swiper
        :modules="[SwiperAutoplay, SwiperNavigation]"
        :slides-per-view="1"
        :loop="true"
        :autoplay="{
          delay: 4000,
          disableOnInteraction: true,
        }"
      >
        <SwiperSlide>
          <div class="w-full items-center px-4">
            <NuxtImg
              format="webp"
              src="assets/gallery-item-1.jpg"
              class="w-full object-cover object-center rounded-lg"
              alt="imagem da galeria de fotos"
              loading="lazy"
              width="540"
              height="474"
            />
            <div class="flex pt-3 overflow-hidden">
              <NuxtImg
                format="webp"
                src="assets/gallery-item-2-2.jpg"
                class="w-1/2 pr-1 object-cover object-center rounded-lg"
                alt="imagem da galeria de fotos"
                loading="lazy"
                width="255"
                height="374"
              />
              <NuxtImg
                format="webp"
                src="assets/gallery-item-3-2.jpg"
                class="w-1/2 pl-1 object-cover object-center rounded-lg"
                alt="imagem da galeria de fotos"
                loading="lazy"
                width="255"
                height="374"
              />
            </div>
          </div>
        </SwiperSlide>
        <SwiperSlide>
          <div class="w-full items-center px-4">
            <div class="flex pb-3 justify-center">
              <NuxtImg
                format="webp"
                src="assets/gallery-item-4-2.jpg"
                class="w-1/2 pr-1 object-cover object-center rounded-lg"
                alt="imagem da galeria de fotos"
                loading="lazy"
                width="255"
                height="374"
              />
              <NuxtImg
                format="webp"
                src="assets/gallery-item-5-2.jpg"
                class="w-1/2 pl-1 object-cover object-center rounded-lg"
                alt="imagem da galeria de fotos"
                loading="lazy"
                width="255"
                height="374"
              />
            </div>
            <NuxtImg
              format="webp"
              src="assets/gallery-item-6.jpg"
              class="w-full object-cover object-center rounded-lg"
              alt="imagem da galeria de fotos"
              loading="lazy"
              width="540"
              height="474"
            />
          </div>
        </SwiperSlide>
      </Swiper>
    </div>
  </section>
</template>

<script setup lang="ts">
</script>