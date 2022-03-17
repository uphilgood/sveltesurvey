
<svelte:options tag="survey-form" />
<script>
	import { createForm } from "svelte-forms-lib";
	export let app = 'someapp';
	export let user = 'someuser';

	const SURVEY_QUESTIONS = [
		{
			id: "nps_value",
			text: "On a scale from 0 (Not at all likely) to 10 (Extremely likely), how likely are you to recommend Backstage to a colleague?",
			type: "nps",
		},
		{
			id: "comments",
			text: "What are the primary reasons for the score you gave us?",
			type: "text",
			isRequired: false,
		},
	];

	const NPS_SCORE = ['0','1', '2','3', '4', '5', '6', '7', '8', '9', '10'];

	const { form, handleChange, handleSubmit } = createForm({
		initialValues: {
			nps_value: undefined,
			comments: "",
		},
		onSubmit: (values) => {
			const finalObj = { ...values, app, user}
			alert(JSON.stringify(finalObj));
		},
	});
</script>

  <style>
    /*
      To avoid css naming collisions, css classes are automatically hashed.
      Unfortunately that means if we want to customize the helper component e.g. the <Field/> component;
      we will need to use the :global(.class-name) and pass .class-name down as a class prop

      See https://github.com/sveltejs/svelte/issues/2870 for about this issue
    */
	.form {
		font-family: monospace;
		max-width: 500px;
		margin: 10px auto;
		padding: 16px;
		background: #1e2145;
	}
    .form-field {
      font-family: monospace;
      padding: 18px;
      border: none;
      border-radius: 0;
      background: #f7f7f7;
      color: black;
    }
    .form-field:focus {
      border-color: #5be2a9;
      box-shadow: 0 0 0 5px #5be2a9;
    }
    .form-label {
      color: #f7f7f7;
    }
  </style>

<form class='form' on:submit={handleSubmit}>
	{#each SURVEY_QUESTIONS as question}
		<label class='form-label' for={question.id}>
			{question.text}
		</label>
		<br />
		{#if question.id === "nps_value"}
			<select class='form-field'
				id={question.id}
				name={question.id}
				on:change={handleChange}
				bind:value={$form.nps_value}
			>
				{#each NPS_SCORE as score}
					<option>{score}</option>
				{/each}
			</select>
		{:else}
			<input class='form-field'
				id={question.id}
				name={question.id}
				on:change={handleChange}
				bind:value={$form.comments}
			/>
		{/if}
		<br/>
	{/each}


	<button type="submit">Submit</button>
</form>
