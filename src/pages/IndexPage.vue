<template>
<TitleIndexComponent/>
<CarouselComponent/>

</template>

<script setup lang="ts">
import { Todo, Meta } from 'components/models';
import CarouselComponent from 'src/components/CarouselComponent.vue';
import TitleIndexComponent from 'src/components/TitleIndexComponent.vue';
import { ref } from 'vue';


const items = ref([
      {
        image: "https://example.com/image1.jpg",
        caption: "Slide 1",
        link: "https://example.com"
      },
      {
        image: "https://example.com/image2.jpg",
        caption: "Slide 2",
        link: "https://example.com"
      },
      {
        image: "https://example.com/image3.jpg",
        caption: "Slide 3",
        link: "https://example.com"
      }
    ]);

const meta = ref<Meta>({
  totalCount: 1200,
});
</script>
