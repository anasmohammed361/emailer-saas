<script lang="ts">
	import * as Form from '$lib/components/ui/form';

	import { Input } from '$lib/components/ui/input';
	import * as Select from '$lib/components/ui/select';
	import { type SuperValidated, type Infer, superForm } from 'sveltekit-superforms';
	import { zodClient } from 'sveltekit-superforms/adapters';
	import { formSchema, type FormSchema } from './schema';
	import TagInput from '@/components/ui/tag-input/tag-input.svelte';
	import { Textarea } from '@/components/ui/textarea';
	import { Separator } from '$lib/components/ui/separator';
	import { Button } from '@/components/ui/button';
	import Checkbox from '@/components/ui/checkbox/checkbox.svelte';
	import * as Alert from '$lib/components/ui/alert/index.js';
	import Icon from '@iconify/svelte';

	export let data: SuperValidated<Infer<FormSchema>>;
	const handleSubmit = async () => {
		$formData.attachments = $formData.attachments.filter((url) => url);
		$formData.smtpPort = Number($formData.smtpPort);
		
		if ($formData.provider === 'google') {
			$formData.smtpHost = 'smtp.gmail.com';
			$formData.smtpPort = 465;
			$formData.ssl = true;
		}
		submit();
	};

	const form = superForm(data, {
		validators: zodClient(formSchema)
	});

	function removeUrlByIndex(index: number) {
		$formData.attachments = $formData.attachments.filter((_, i) => i !== index);
	}

	function addUrl() {
		$formData.attachments = [...$formData.attachments, ''];
	}
	const { form: formData, enhance, submit } = form;
	$: selectedEmail = $formData.provider
		? {
				label: $formData.provider,
				value: $formData.provider
			}
		: undefined;
	$: selectedType = $formData.contentType
		? {
				label: $formData.contentType,
				value: $formData.contentType
			}
		: undefined;
	$formData.attachments = $formData.attachments ?? [''];
</script>

<form method="POST" use:enhance>
	<Form.Field {form} name="fromAddress">
		<Form.Control let:attrs>
			<Form.Label>From Address</Form.Label>
			<Input
				placeholder={`"My Name" <example@mail.com>`}
				{...attrs}
				bind:value={$formData.fromAddress}
			/>
		</Form.Control>
		<Form.FieldErrors />
	</Form.Field>
	<Form.Field {form} name="toAddresses">
		<Form.Control let:attrs>
			<Form.Label>To Address</Form.Label>
			<TagInput {form} bind:inputTags={$formData.toAddresses} />
		</Form.Control>
		<Form.Description>Press Enter after typing in your email.</Form.Description>
		<Form.FieldErrors />
	</Form.Field>

	<Form.Field {form} name="subject">
		<Form.Control let:attrs>
			<Form.Label>Subject</Form.Label>
			<Input
				{...attrs}
				placeholder="Your Email Subject goes here."
				bind:value={$formData.subject}
			/>
		</Form.Control>

		<Form.FieldErrors />
	</Form.Field>
	<Form.Field {form} name="contentType">
		<Form.Control let:attrs>
			<Form.Label>Content Type</Form.Label>
			<Select.Root
				selected={selectedType}
				onSelectedChange={(v) => {
					v && ($formData.contentType = v.value);
				}}
			>
				<Select.Trigger {...attrs}>
					<Select.Value class="capitalize" placeholder="Select a verified email to display" />
				</Select.Trigger>
				<Select.Content>
					<Select.Item value="text" label="Text" />
					<Select.Item value="html" label="Html" />
				</Select.Content>
			</Select.Root>
			<input hidden bind:value={$formData.contentType} name={attrs.name} />
		</Form.Control>
		<Form.Description
			>Choose if you wanna send <strong>text</strong> or <strong>html</strong>.</Form.Description
		>
		<Form.FieldErrors />
	</Form.Field>
	<Form.Field {form} name="content">
		<Form.Control let:attrs>
			<Form.Label>Email Body</Form.Label>
			<Textarea
				placeholder="Your Email Body goes here..."
				{...attrs}
				bind:value={$formData.content}
			/>
		</Form.Control>
		<Form.FieldErrors />
	</Form.Field>
	<Form.FormFieldset {form} name="attachments">
		<Form.Control>
			<Form.Label>Attachments</Form.Label>
			{#each $formData.attachments ?? [] as _, i}
				<Form.ElementField {form} name="attachments[{i}]">
					<Form.Control let:attrs>
						<dir class="flex gap-3">
							<Input
								class="grow"
								{...attrs}
								placeholder="Paste url of your documents to attach."
								bind:value={$formData.attachments[i]}
							/>
							<Button size="icon" variant="outline" on:click={() => removeUrlByIndex(i)}>
								<Icon icon="fluent:delete-32-filled" class="size-6" />
							</Button>
						</dir>
						<Form.FieldErrors />
					</Form.Control>
				</Form.ElementField>
			{/each}
		</Form.Control>
		<Form.Description>Attach files to your email.</Form.Description>
		<Form.FieldErrors />
		<div class="flex justify-end">
			<Button size="icon" variant="outline" on:click={addUrl}>
				<Icon icon="gg:add" class="size-6" />
			</Button>
		</div>
	</Form.FormFieldset>

	<Separator class="my-10" />
	<Form.Field {form} name="provider">
		<Form.Control let:attrs>
			<Form.Label>Provider</Form.Label>
			<Select.Root
				selected={selectedEmail}
				onSelectedChange={(v) => {
					v && ($formData.provider = v.value);
				}}
			>
				<Select.Trigger {...attrs}>
					<Select.Value class="capitalize" placeholder="Select a Provider" />
				</Select.Trigger>
				<Select.Content>
					<Select.Item value="google" label="Google" />
					<Select.Item value="custom" label="Custom" />
				</Select.Content>
			</Select.Root>
			<input hidden bind:value={$formData.provider} name={attrs.name} />
		</Form.Control>
		<Form.Description>Choose your SMTP Provider.</Form.Description>
		<Form.FieldErrors />
	</Form.Field>
	{#if $formData.provider === 'google'}
		<Form.Field {form} name="smtpUserName">
			<Form.Control let:attrs>
				<Form.Label>SMTP Username</Form.Label>
				<Input
					{...attrs}
					placeholder="Your SMTP Email Address..."
					bind:value={$formData.smtpUserName}
				/>
			</Form.Control>
			<!-- <Form.Description>This is your public display name.</Form.Description> -->
			<Form.FieldErrors />
		</Form.Field>
		<Form.Field {form} name="smtpPassword">
			<Form.Control let:attrs>
				<Form.Label>SMTP Password</Form.Label>
				<Input
					{...attrs}
					placeholder="Your Email Subject goes here."
					bind:value={$formData.smtpPassword}
				/>
			</Form.Control>
			<Form.Description
				>Your Google <a
					href="https://support.google.com/accounts/answer/185833?hl=en"
					target="_blank">App Password</a
				> goes here.</Form.Description
			>
			<Form.FieldErrors />
		</Form.Field>
	{:else if $formData.provider === 'custom'}
		<Form.Field {form} name="smtpHost">
			<Form.Control let:attrs>
				<Form.Label>SMTP Host</Form.Label>
				<Input {...attrs} placeholder="Your SMTP Host..." bind:value={$formData.smtpHost} />
			</Form.Control>
			<!-- <Form.Description>This is your public display name.</Form.Description> -->
			<Form.FieldErrors />
		</Form.Field>
		<Form.Field {form} name="smtpUserName">
			<Form.Control let:attrs>
				<Form.Label>SMTP Username</Form.Label>
				<Input {...attrs} placeholder="Your SMTP Username..." bind:value={$formData.smtpUserName} />
			</Form.Control>
			<!-- <Form.Description>This is your public display name.</Form.Description> -->
			<Form.FieldErrors />
		</Form.Field>
		<Form.Field {form} name="smtpPassword">
			<Form.Control let:attrs>
				<Form.Label>SMTP Password</Form.Label>
				<Input
					{...attrs}
					placeholder="Your Email Subject goes here."
					bind:value={$formData.smtpPassword}
					type="password"
				/>
			</Form.Control>
			<Form.Description>We don't store any of your passwords.</Form.Description>
			<Form.FieldErrors />
		</Form.Field>
		<Form.Field {form} name="smtpPort">
			<Form.Control let:attrs>
				<Form.Label>SMTP Port</Form.Label>
				<Input
					{...attrs}
					type="number"
					placeholder="Your SMTP Port..."
					bind:value={$formData.smtpPort}
				/>
			</Form.Control>
			<!-- <Form.Description>This is your public display name.</Form.Description> -->
			<Form.FieldErrors />
		</Form.Field>
		<Form.Field {form} name="ssl">
			<Form.Control let:attrs>
				<Checkbox {...attrs} bind:checked={$formData.ssl} />
				<Form.Label>Use SSL</Form.Label>
			</Form.Control>
			<Form.FieldErrors />
		</Form.Field>
	{:else}
		<Alert.Root variant="destructive" class="my-10">
			<Icon icon="zondicons:exclamation-outline" class="size-4" />
			<Alert.Title>No Provider!</Alert.Title>
			<Alert.Description>Please Choose a Provider before configuring SMTP.</Alert.Description>
		</Alert.Root>
	{/if}

	<div class="flex justify-end">
		<Button
			on:click={handleSubmit}
			class="flex cursor-pointer items-center gap-1 rounded-md bg-blue-500 px-4 py-2  font-semibold tracking-widest text-white transition-all duration-300 hover:translate-x-1 hover:gap-1 hover:bg-blue-400"
			>Send
			<svg
				class="h-5 w-5"
				stroke="currentColor"
				stroke-width="1.5"
				viewBox="0 0 24 24"
				fill="none"
				xmlns="http://www.w3.org/2000/svg"
			>
				<path
					d="M6 12 3.269 3.125A59.769 59.769 0 0 1 21.485 12 59.768 59.768 0 0 1 3.27 20.875L5.999 12Zm0 0h7.5"
					stroke-linejoin="round"
					stroke-linecap="round"
				></path>
			</svg></Button
		>
	</div>
</form>
