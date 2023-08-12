<script>
	export let name;
	let fileContent = '';
    let incidents = [];
    let incidentsByYear = {};  // New structure: { '2023': [{date, title, description}, ...], ... }
    let selectedIncidents = [];

    function readFile(event) {
        const file = event.target.files[0];
        if (file) {
            const reader = new FileReader();

            reader.onload = (e) => {
                fileContent = e.target.result;
                processFileContent();
            };

            reader.readAsText(file);
        }
    }

    function processFileContent() {
        const lines = fileContent.split('\n').map(line => line.trim()).filter(line => line);
        let currentIncident = null;

        lines.forEach((line, index) => {
            if (/\d{2}\/\d{2}\/\d{4}/.test(line)) {
                if (currentIncident) {
                    const year = currentIncident.date.split('/')[2];
                    if (!incidentsByYear[year]) incidentsByYear[year] = [];
                    incidentsByYear[year].push(currentIncident);
                }
                const [date, ...titleParts] = line.split(' ');
                currentIncident = {
                    date: date,
                    title: titleParts.join(' '),
                    description: ''
                };
            } else if (currentIncident) {
                currentIncident.description += line + ' ';
            }

            if (index === lines.length - 1 && currentIncident) {
                const year = currentIncident.date.split('/')[2];
                if (!incidentsByYear[year]) incidentsByYear[year] = [];
                incidentsByYear[year].push(currentIncident);
            }
        });
    }

    function selectYear(year) {
        selectedIncidents = incidentsByYear[year].map(incident => `${incident.date} - ${incident.title}\n${incident.description}`).join('\n\n');
    }
</script>

<input type="file" on:change={readFile} accept=".txt" />

{#each incidents as incident (incident.date)}
    <h2>{incident.date} - {incident.title}</h2>
    <p>{incident.description}</p>
{/each}

<main>
	<h1>Welcome to the Aviation Incidents App</h1>
	<input type="file" on:change={readFile} accept=".txt" />
	{#each incidents as incident (incident.date)}
		<h2>{incident.date} - {incident.title}</h2>
		<p>{incident.description}</p>
	{/each}
	<section>
		{#each Object.keys(incidents) as year}
			<button on:click={() => (fileContent = incidents[year])}>
				{year}
			</button>
		{/each}
	</section>
	<textarea readonly bind:value={fileContent}></textarea>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>