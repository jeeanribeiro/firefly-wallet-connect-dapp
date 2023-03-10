<script>
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';

	import { EthereumClient, w3mConnectors, w3mProvider } from '@web3modal/ethereum'
	import { Web3Modal } from '@web3modal/html'
	import { configureChains, createClient } from '@wagmi/core'
	import { arbitrum, mainnet, polygon } from '@wagmi/core/chains'

	const chains = [arbitrum, mainnet, polygon]
	const projectId = '760c10756afd8450d4d96b55ef0748ec'

	const { provider } = configureChains(chains, [w3mProvider({ projectId })])
	const wagmiClient = createClient({
		autoConnect: true,
		connectors: w3mConnectors({ projectId, version: 1, chains }),
		provider,
	})
	const ethereumClient = new EthereumClient(wagmiClient, chains)
	const web3modal = new Web3Modal({ 
		// @ts-ignore
		projectId: undefined, 
		desktopWallets: [
			{
				id: "Firefly",
				name: "Firefly",
				links: {
					native: "firefly-alpha://connect-",
					universal: "string",
				},
			},
		],
		enableExplorer: false,
		}
		, ethereumClient)
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		to your new<br />SvelteKit app
	</h1>

	<h2>
		try editing <strong>src/routes/+page.svelte</strong>
	</h2>

	<Counter />

	<w3m-core-button></w3m-core-button>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
