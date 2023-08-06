<script lang="ts">
	import type { KMLPipeline } from 'kml-pipe-ts';
	import { onMount } from 'svelte';

	let pipe: KMLPipeline;
	let files: FileList;
	let videoSource: HTMLVideoElement;
	let outputData: any[] = [];
	let crop: number[] = [0, 0, 0, 0];

	onMount(async () => {
		let { KMLPipeline } = await import('kml-pipe-ts');
		pipe = new KMLPipeline('Outside LLMs Trainer', 1, '79705c77-f57b-449d-b856-03138e8859a7');
		await pipe.initialize();
	});
	$: files && files.length > 0 && loadVideo(files[0]);
	const loadVideo = async (video: File) => {
		let url = URL.createObjectURL(video);
		console.log(url);
		videoSource.srcObject = null;
		videoSource.src = url;
		await videoSource.play();
		crop = [0, 0, videoSource.videoHeight, videoSource.videoWidth];
		processFrame();
	};
	const processFrame = async () => {
		videoSource.pause();
		let outputs = await pipe.execute([videoSource, crop]);
		console.log(outputs[2]);
		console.log('pushed data ' + JSON.stringify(outputs[0].value));
		outputData.push({ data: outputs[0].value, time: Date.now() });
		outputData = outputData;
		console.log(crop);
		//crop = outputs[1].value as number[];
		videoSource.requestVideoFrameCallback(processFrame);
		videoSource.play();
	};
</script>

<div class="h-full w-full">
	<input type="file" bind:files />
	<p>{outputData.length}</p>
	<a href={`data:text/json;charset=utf-8,${JSON.stringify(outputData)}`} download="data.json"
		>Download Data</a
	>
	<video id="webcam" autoplay class="rounded-lg shadow-lg" bind:this={videoSource} />
</div>
