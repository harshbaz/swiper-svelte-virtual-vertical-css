<script lang="ts">
	import { onMount } from 'svelte';
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

	let virtualSlides = ['https://picsum.photos/id/1/200/300'];

	onMount(() => {
		setTimeout(() => {
			virtualSlides = generateImageUrls(20);
		}, 1000);
	});

	$: console.log({ virtualSlides });
</script>

<main class="border h-screen w-full">
	<Swiper
		modules={[Virtual]}
		slidesPerView={1}
		spaceBetween={50}
		cssMode
		class="h-full w-full"
		direction="vertical"
		virtual={{ slides: virtualSlides, addSlidesBefore: 3, addSlidesAfter: 3 }}
		let:virtualData={{ slides, from }}
		on:slideChange={handleChange}
	>
		{#each slides as slide, index (from + index)}
			<SwiperSlide
				class="h-full border border-red-500 snap-always w-full"
				virtualIndex={from + index}
			>
				<div class="h-full flex items-center justify-center w-full">
					<pre
						class="text-bold absolute z-5 top-0 left-0">totalSlides: {slides.length}, from: {from}, index: {index}, currentSlide: {currentSlideIndex}</pre>
					<img alt="im" class="h-1/2 px-10" src={slide} />
				</div>
			</SwiperSlide>
		{/each}
	</Swiper>
</main>
