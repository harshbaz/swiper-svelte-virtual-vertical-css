<script lang="ts">
	import { Swiper, SwiperSlide } from 'swiper/svelte';
	import 'swiper/css';
	let currentSlideIndex = 0;

	function handleChange(e: CustomEvent) {
		currentSlideIndex = e.detail[0].realIndex;
	}

	function generateImageUrls(count: number) {
		return new Array(count).fill(0).map((_, i) => `https://picsum.photos/id/${i + 1}/200/300`);
	}

	let virtualSlides = generateImageUrls(20);
</script>

<main class="border h-screen w-full">
	<Swiper
		slidesPerView={1}
		spaceBetween={50}
		cssMode
		observer
		direction="vertical"
		class="h-full w-full"
		on:slideChange={handleChange}
	>
		{#each virtualSlides as slide, index (index)}
			<SwiperSlide
				class="h-full border border-red-500 snap-always flex items-center justify-center w-full"
			>
				<div class="h-full flex items-center justify-center w-full">
					<pre
						class="text-bold absolute z-5 top-0 left-0">virtualSlides: {virtualSlides.length}, currentSlide: {currentSlideIndex}</pre>
					<img alt="im" class="h-1/2 px-10" src={slide} />
				</div>
			</SwiperSlide>
		{/each}
	</Swiper>
</main>
