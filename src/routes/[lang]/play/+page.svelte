<script lang="ts">
	import type { KMLPipeline } from 'kml-pipe-ts';
	import { onMount } from 'svelte';

	let pipe: KMLPipeline;
	let videoSource: HTMLVideoElement;
	let canvas: HTMLCanvasElement;
	let processing = false;
	let data: any[] = [];
	let startTime = 0;
	let loading = false;

	onMount(async () => {
		let { KMLPipeline } = await import('kml-pipe-ts');
		pipe = new KMLPipeline('Outside LLMs Runtime', 1, '79705c77-f57b-449d-b856-03138e8859a7');
		await pipe.initialize();
		await loadTrainerData();
		await startWebcam();
	});
	const startWebcam = async () => {
		const stream = await navigator.mediaDevices.getUserMedia({ video: true });
		videoSource.srcObject = stream;
		await videoSource.play();
		canvas.height = videoSource.clientHeight;
		canvas.width = videoSource.clientWidth;
		startTime = Date.now();
		videoSource.requestVideoFrameCallback(processFrame);
		loading = false;
	};
	const processFrame = async () => {
		if (!processing) {
			processing = true;
			let time = Date.now();
			let frame = findCorrelatedFrame(time - startTime);
			if (frame) {
				canvas.getContext('2d')?.clearRect(0, 0, videoSource.clientWidth, videoSource.clientHeight);
				let outputs = await pipe.execute([videoSource, canvas, frame.data]);
				console.log('similarity: ' + JSON.stringify(outputs[0].value));
			}
			processing = false;
			videoSource.requestVideoFrameCallback(processFrame);
		}
	};
	const loadTrainerData = async () => {
		let response = await fetch('/data.json');
		data = await response.json();
		let start = data[0].time;
		data = data.map((d, i) => ({ ...d, time: i * 1000 * (1 / 24) }));
	};
	const findCorrelatedFrame = (time: number) => {
		for (let i = 0; i < data.length; i++) {
			if (data[i].time > time) {
				return data[i - 1];
			}
		}
	};
</script>

<div class="h-full w-full">
	<div class="relative">
		<video id="webcam" autoplay class="rounded-lg shadow-lg" bind:this={videoSource} />
		<canvas id="canvas" class="absolute left-0 top-0 z-10 overflow-hidden" bind:this={canvas} />
	</div>
</div>
