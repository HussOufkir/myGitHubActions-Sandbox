# myGitHubActions-Sandbox
(__be careful with the information you will find here, it's just a sandbox__)

## Contexts
Contexts are a way to access information about workflow runs, variables, runner environments, jobs, and steps. Each context is an object that contains properties, which can be strings or other objects.

More info : https://docs.github.com/en/actions/learn-github-actions/contexts

To use contexts : 
```
${{ <context> }}
```

## How-to
### Store an output

Old method will be disabled on 1st june 2023.

Example :

```
echo "::set-output name=myOutput::myOutputValue123"
```

New method since runner version 2.298.2 : https://github.blog/changelog/2022-10-11-github-actions-deprecating-save-state-and-set-output-commands/

Example :

```
echo "<myOutput>=<myOutputValue123>" >> "$GITHUB_OUTPUT"
```

### Reference a variable

```
${{ env.myVar }} =/= ${ myVar }
```


## Workflow syntax

```yml
name:    # [optional] Name of the workflow. Ex : myCICD.
run-name:    # [optional] Name of the running workflow. Some env value can be added. Ex : workflow triggered by @${{ github.triggering_actor }}.
on:    # [required(with1child)] How the workflow will be triggered. More info : https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows
	<event_name>:    # [optional] Event that trigger the execution of the workflow. Ex : pull_request / push / [push, fork] / ...
		types:    # [optional] Activity types. Ex : opened / edited / ... 
		branches:  (for pull_request|pull_request_target|push)    # [optional] Branches to consider. Ex : master, develop, feature/* (just 1 sub), feature/** (more that 1 sub), ...
		branches-ignore:   (for pull_request|pull_request_target|push)    # [optional] Branches to ignore. Ex : release/*, hotfix/*, ...
		tags:   (for push)    # [optional] Tags to consider. Ex : v1.0 / v2.0 / ...
		tags-ignore:   (for push)    # [optional] Tags to ignore. Ex : rc/*, ...
		paths:   (for pull_request|pull_request_target)    # [optional] File paths to consider. Ex : docs/*, src/index.js, ...
		paths-ignore:   (for push|pull_request|pull_request_target)    # [optional] File paths to ignore. Ex : docs/*.md, *.json, ...
	schedule:    # [optional] To plan the execution by one or more schedule.
		cron:    # [required] Plan with cron format. (0-59)(0-23)(1-31)(1-12)(0-6). Ex : '0 0 * * SAT' (midnight on saturday) / '15 2-4 * * 1,3' (every 15 min at 2,3,4 hour each SUN and TUE) / ...
	workflow_run:    # [optional] To be triggered by another workflow.
		workflows:    # [required] Array of workflows to consider. Ex : [build, deploy, myWorkflow1] / myWorkFlow2 / ...
		types:    # [optional] Type of event (requested, in_progress, completed).
		branches:    # [optional] Branches to consider. Ex : master, develop, feature/*, ...
		branches-ignore:    # [optional] Branches to ignore. Ex : release/*, hotfix/*, ...
	workflow_dispatch:    # [optional] To be triggered manually.
		inputs:    # [optional] Required inputs.
			<input_id>:    # [required] Input to use in the workflow.
				required:     # [required] Boolean that indicate if the input is required.
				type:    # [required] Type of data. Must be one of: boolean, choice, number, string.
				description:    # [optional] Description of the input.
				default:    # [optional] Default value.
				options:    (for type:choice)    # [required] Available choice for the user.
	workflow_call:    # [optionnel] Evenement de lancement d'un autre workflow a partir du workflow courant.
		inputs:    # Inputs requis pour le workflow appele. Ex : { "param1": "valeur1", "param2": "valeur2" }
			<input_id>:
				type:
		outputs:    # Outputs du workflow appele. Ex : { "output1": ${{ steps.job1.outputs.output1 }}, "output2": ${{ steps.job2.outputs.output2 }} }
		secrets:    # [optionnel] Liste des secrets utilises dans le workflow. Ex : { "SECRET_API_KEY": ${{ secrets.API_KEY }} }
			<secret_id>:
				required:
permissions:    # [optionnel] Permissions requises pour executer le workflow. Ex : "write" pour la creation de branches.
env:    # [optionnel] Variables d'environnement globales.
defaults:    # [optionnel] Definition des valeurs par defaut.
	run:    # [optionnel] Valeurs par defaut pour les actions de type "run".
concurrency:    # [optionnel] Definition de la limite de parallelisme pour le workflow.
jobs:    # [requis] Liste des jobs a executer.
	<job_id>:    # [requis] Identifiant unique du job. Ex : test, build, deploy.
		name:      # [optionnel] Un nom convivial pour le job.
		permissions:    # [optionnel] Les autorisations nécessaires pour exécuter le job, comme les clés SSH ou les jetons d'accès.
		needs:     # [optionnel] Liste des jobs qui doivent être terminés avec succès avant que celui-ci ne soit exécuté.
		if:        # [optionnel] Une expression booléenne qui contrôle si le job est exécuté ou ignoré. Si l'expression est vraie, le job sera exécuté ; sinon, il sera ignoré.
		runs-on:   # [optionnel] Le type de machine à utiliser pour exécuter le job, comme "ubuntu-latest" ou "windows-2019".
		environment:    # [optionnel] Les variables d'environnement à définir pour le job.
		concurrency:    # [optionnel] La limite de parallélisme pour le job.
		outputs:   # [optionnel] Les fichiers ou les valeurs à produire en sortie du job.
		env:       # [optionnel] Les variables d'environnement à définir pour toutes les étapes du job.
		defaults:  # [optionnel] Les valeurs par défaut à utiliser pour toutes les étapes du job.
			run:  
		steps:     # [requis] Les étapes à exécuter dans le job.
			<steps_name>:
				id:    # Identifiant unique de l'étape.
				if:    # Expression qui détermine si l'étape doit être exécutée ou non, en fonction des variables d'environnement définies.
				name:    # Nom de l'étape.
				uses:    # [requis ?] URL de l'action ou du template à utiliser pour cette étape.
				run:    # [requis ?] Commande à exécuter dans un environnement shell pour cette étape.
				shell:    # Shell à utiliser pour exécuter la commande pour cette étape.
				with:    # Options à utiliser pour cette étape.
					args:    # Arguments à passer à l'action ou au template utilisé pour cette étape.
					entrypoint:    # Point d'entrée à utiliser pour l'action ou le template utilisé pour cette étape.
				env:    # Variables d'environnement à définir pour cette étape.
				continue-on-error:    # Spécifie si le workflow doit continuer en cas d'erreur dans cette étape.
				timeout-minutes:    # Temps limite pour l'exécution de cette étape en minutes.
		timeout-minutes:  # [optionnel] La durée maximale autorisée pour l'exécution du job, en minutes.
		strategy:    # [optionnel] La stratégie à utiliser pour exécuter plusieurs instances du job en parallèle.
			matrix:
				include:
				exclude:
		fail-fast:  # [optionnel] Si cette option est activée, l'exécution de toutes les instances du job sera interrompue si l'une d'entre elles échoue.
		max-parallel:   # [optionnel] Le nombre maximal d'instances du job qui peuvent être exécutées simultanément.
		continue-on-error:  # [optionnel] Si cette option est activée, l'exécution des étapes suivantes se poursuivra même si une étape échoue.
		container:   # [optionnel] Les paramètres du conteneur à utiliser pour exécuter le job.
			image:
			credentials:
			env:
			ports:
			volumes:
			options:
		services:   # [optionnel] Les services à utiliser pour exécuter le job.
			<service_id>:
				image:
				credentials:
				env:
				ports:
				volumes:
				options:
		uses:      # [optionnel] L'action ou le template à utiliser pour exécuter le job.
		with:      # [optionnel] Les entrées à fournir à l'action ou au template utilisé.
			<input_id>:
		secrets:   # [optionnel] Les secrets à utiliser pour exécuter le job.
            inherit:    # [optionnel] Si cette option est activée, les secrets du repository seront automatiquement inclus dans le job.
            <secret_id>:    # [optionnel] Le nom du secret à utiliser dans le job. Le secret doit être défini dans les paramètres du repository.
```
