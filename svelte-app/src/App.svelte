<script>
	let fileContent = '';
    let incidentsByYear = {};
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
	
	let displayedIncidents = '';

	function selectYear(year) {
		selectedIncidents = incidentsByYear[year];
		displayedIncidents = selectedIncidents.map(incident => `${incident.date} - ${incident.title}\n${incident.description}`).join('\n\n');
	}
</script>

<input type="file" on:change={readFile} accept=".txt" />

{#each selectedIncidents as incident (incident.date)}
    <h2>{incident.date} - {incident.title}</h2>
    <p>{incident.description}</p>
{/each}

<main>
	<h1>Welcome to the Aviation Incidents App</h1>
	<input type="file" on:change={readFile} accept=".txt" />
	
	<section>
		{#each Object.keys(incidentsByYear) as year}
			<button on:click={() => selectYear(year)}>
				{year}
			</button>
		{/each}
	</section>
	
	<textarea readonly bind:value={selectedIncidents}></textarea>
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