<script lang="ts">
	import { Virtual } from 'swiper';
	import 'swiper/css';

	import { Swiper, SwiperSlide } from 'swiper/svelte';

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
	<pre class="text-bold fixed z-5 top-0 left-0">currentSlide: {currentSlideIndex}</pre>
	<Swiper
		modules={[Virtual]}
		slidesPerView={1}
		spaceBetween={50}
		cssMode
		direction="vertical"
		virtual={{ slides: virtualSlides, addSlidesBefore: 3, addSlidesAfter: 3 }}
		let:virtualData={{ slides, offset, from }}
		on:slideChange={handleChange}
	>
		{#each slides as slide, index (from + index)}
			<SwiperSlide
				class="h-full border border-red-500 snap-always flex items-center justify-center w-full"
				virtualIndex={from + index}
				style={`top: ${offset}px`}
			>
				<img alt="im" class="w-1/2" src={slide} />
			</SwiperSlide>
		{/each}
	</Swiper>
</main>
