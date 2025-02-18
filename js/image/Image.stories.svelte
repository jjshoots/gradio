<script lang="ts">
	import { Meta, Template, Story } from "@storybook/addon-svelte-csf";
	import StaticImage from "./Index.svelte";
	import { userEvent, within } from "@storybook/testing-library";
</script>

<Meta title="Components/Image" component={Image} />

<Template let:args>
	<div
		class="image-container"
		style="width: 300px; position: relative;border-radius: var(--radius-lg);overflow: hidden;"
	>
		<StaticImage {...args} />
	</div>
</Template>

<Story
	name="static with label and download button"
	args={{
		value: {
			path: "https://gradio-builds.s3.amazonaws.com/demo-files/ghepardo-primo-piano.jpg",
			url: "https://gradio-builds.s3.amazonaws.com/demo-files/ghepardo-primo-piano.jpg",
			orig_name: "cheetah.jpg"
		},
		show_label: true,
		show_download_button: true
	}}
/>

<Story
	name="static with no label or download button"
	args={{
		value: {
			path: "https://gradio-builds.s3.amazonaws.com/demo-files/ghepardo-primo-piano.jpg",
			url: "https://gradio-builds.s3.amazonaws.com/demo-files/ghepardo-primo-piano.jpg",
			orig_name: "cheetah.jpg"
		},
		show_label: false,
		show_download_button: false
	}}
/>

<Story
	name="interactive with upload, clipboard, and webcam"
	args={{
		sources: ["upload", "clipboard", "webcam"],
		value: {
			path: "https://gradio-builds.s3.amazonaws.com/demo-files/ghepardo-primo-piano.jpg",
			url: "https://gradio-builds.s3.amazonaws.com/demo-files/ghepardo-primo-piano.jpg",
			orig_name: "cheetah.jpg"
		},
		show_label: false,
		show_download_button: false,
		interactive: true
	}}
	play={async ({ canvasElement }) => {
		const canvas = within(canvasElement);

		const webcamButton = await canvas.findByLabelText("Capture from camera");
		userEvent.click(webcamButton);

		userEvent.click(await canvas.findByTitle("select video source"));
		userEvent.click(await canvas.findByLabelText("select source"));
		userEvent.click(await canvas.findByLabelText("Upload file"));
		userEvent.click(await canvas.findByLabelText("Paste from clipboard"));
	}}
/>

<Story
	name="interactive with webcam"
	args={{
		sources: ["webcam"],
		show_download_button: true,
		interactive: true
	}}
/>

<Story
	name="interactive with clipboard"
	args={{
		sources: ["clipboard"],
		show_download_button: true,
		interactive: true
	}}
/>

<Story
	name="interactive webcam with streaming"
	args={{
		sources: ["webcam"],
		show_download_button: true,
		interactive: true,
		value: {
			path: "https://gradio-builds.s3.amazonaws.com/demo-files/ghepardo-primo-piano.jpg",
			url: "https://gradio-builds.s3.amazonaws.com/demo-files/ghepardo-primo-piano.jpg",
			orig_name: "cheetah.jpg"
		},
		streaming: true
	}}
/>
