<script context="module">
	export function load({ fetch, page }) {
		console.log('Load run', page.query.toString())

		return fetch(`${page.path}test.json?${page.query.toString()}`)
			.then((json) => {
				return json
			})
			.catch(() => {
				return {
					status: 500,
					error: new Error(`Could not load required files`),
				}
			})
	}
</script>

<script>
	import { goto } from '$app/navigation'
	import { page } from '$app/stores'

	export function updateQuery(query, changes) {
		for (const [key, value] of Object.entries(changes)) {
			if (value === '' || value === null) query.delete(key)
			else query.set(key, value)
		}

		goto(`?${query.toString().replace(/%7C/g, '|')}`, { keepfocus: true, replaceState: true, noscroll: true })
	}
</script>

<h1><code>goto</code> load issue</h1>
<p>Alternate clicking between the two buttons and see how load is only called for one of the two in the devtools console.</p>

<button on:click={() => updateQuery($page.query, { test: true })}>Set true</button>
<button on:click={() => updateQuery($page.query, { test: false })}>Set false</button>
