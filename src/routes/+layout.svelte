<script>
	import '../app.pcss';
	import '@fontsource-variable/inter';
	import '@fontsource-variable/bricolage-grotesque';
	import { Toaster } from '$lib/components/ui/sonner';
	import { ModeWatcher } from 'mode-watcher';
	import SvelteSeo from 'svelte-seo';
	import { partytownSnippet } from '@builder.io/partytown/integration';
	import { dev } from '$app/environment';
</script>

<svelte:head>
	{@html '<script>' + partytownSnippet() + '</script>'}
	{#if !dev}
		<script
			type="text/partytown"
			src="https://analytics.webzenith.tech/script.js"
			data-website-id="44d7d1bd-82c6-4030-9d15-82a4533d8d4a"
		></script>
	{/if}
</svelte:head>

<SvelteSeo
	title="ZenithMails | Send Automated Mails"
	description="Elevate your email game with Zenith Mails, the open-source email automation tool. Send personalized emails, automate campaigns, and track performance with ease."
	canonical="https://mail.webzneith.tech/"
	keywords="email automation, webzenith, zenith mails, saas, email marketing, campaigns, open source, automated mails, analytics , email tracking, email performance, email campaigns, email automation tool, email automation software, email automation platform, email automation service, email automation system, email automation tool free, email automation tool open source, email automation tool for developers, email automation tool for marketers, email automation tool for business, email automation tool for startups, email automation tool for small business, email automation tool for saas, email automation tool for ecommerce, email automation tool for agencies, email automation tool for freelancers, email automation tool for bloggers, email automation tool for creators, email automation tool for content creators, email automation tool for digital marketers, email automation tool for growth hackers, email automation tool for product managers, email automation tool for sales, email automation tool for customer success, email automation tool for customer support, email automation tool for customer service, email automation tool for customer experience, email automation tool for customer engagement, email automation tool for customer retention, email automation tool for customer acquisition, email automation tool for customer onboarding, email automation tool for customer lifecycle, email automation tool for customer journey, email automation tool for customer relationship, email automation tool for customer communication, email automation tool for customer feedback, email automation tool for customer surveys, email automation tool for customer reviews, email automation tool for customer testimonials, email automation tool for customer referrals, email automation tool for customer loyalty, email automation tool for customer advocacy, email automation tool for customer experience management, email automation tool for customer relationship management, email automation tool for customer communication management, email automation tool for customer feedback management, email automation tool for customer surveys management, email automation tool for customer reviews management, email automation tool for customer testimonials management, email automation tool for customer referrals management, email automation tool for customer loyalty management, email automation tool for customer advocacy management, email automation tool for customer experience management, email automation tool for customer relationship management, email automation tool for customer communication management, email automation tool for customer feedback management, email automation tool for customer surveys management, email automation tool for customer reviews management, email automation tool for customer testimonials management, email automation tool for customer referrals management, email automation tool for customer loyalty management, email automation tool for customer advocacy management, email automation tool for customer experience management, email automation tool for customer relationship management, email automation tool for customer communication management, email automation tool for customer feedback management, email automation tool for customer surveys management, email automation tool for customer reviews management, email automation tool for customer testimonials management, email automation tool for customer referrals management"
	twitter={{
		card: 'summary_large_image',
		site: '@webzenith99406',
		creator: '@webzenith99406',
		title: 'ZenithMails | Send Automated Mails',
		description:
			'Elevate your email game with Zenith Mails, the open-source email automation tool. Send personalized emails, automate campaigns, and track performance with ease.',
		image: 'https://mail.webzenith.tech/opengraph.png'
	}}
	openGraph={{
		title: 'ZenithMails | Send Automated Mails',
		description:
			'Elevate your email game with Zenith Mails, the open-source email automation tool. Send personalized emails, automate campaigns, and track performance with ease.',
		url: 'https://mail.webzneith.tech/',
		type: 'website',
		images: [
			{
				url: 'https://mail.webzenith.tech/opengraph.png',
				width: 800,
				height: 600,
				alt: 'OpenGraph'
			}
		],
		site_name: 'ZenithMails',
		locale: 'en_US'
	}}
	applicationName={'ZenithMails'}
	jsonLd={{
		'@context': 'https://schema.org',

		'@type': 'WebSite',
		name: 'ZenithMails | Send Automated Mails',
		description:
			'Elevate your email game with Zenith Mails, the open-source email automation tool. Send personalized emails, automate campaigns, and track performance with ease.',
		url: 'https://mail.webzneith.tech/'
	}}
/>

<main class="font-bricolage">
	<slot />
</main>
<Toaster />
<ModeWatcher defaultMode="light" />
