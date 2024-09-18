<script>
  import { getContext, onDestroy } from "svelte";
  import CellDatetime from "../../bb_super_components_shared/src/lib/SuperTableCells/CellDatetime.svelte";
  import SuperButton from "../../bb_super_components_shared/src/lib/SuperButton/SuperButton.svelte";
  import "../../bb_super_components_shared/src/lib/SuperFieldsCommon.css";

  const { styleable, enrichButtonActions } = getContext("sdk");
  const component = getContext("component");
  const allContext = getContext("context");

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const groupLabelPosition = getContext("field-group");
  const labelWidth = getContext("field-group-label-width");
  const groupColumns = getContext("field-group-columns");
  const groupDisabled = getContext("field-group-disabled");
  const formApi = formContext?.formApi;

  export let field;
  export let controlType;
  export let role = "formInpur";

  export let customButtons;
  export let buttons = [];
  export let buttonsQuiet;

  export let label;
  export let span = 6;
  export let placeholder;
  export let defaultValue;
  export let disabled;
  export let readonly;
  export let validation;
  export let template;

  export let icon;
  export let labelPosition = "fieldGroup";
  export let showDirty;
  export let autofocus;

  let formField;
  let formStep;
  let fieldState;
  let fieldApi;
  let fieldSchema;
  let value;
  let cellState;

  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: labelPos =
    groupLabelPosition && labelPosition == "fieldGroup"
      ? groupLabelPosition
      : labelPosition;

  $: formField = formApi?.registerField(
    field,
    "datetime",
    defaultValue,
    disabled,
    readonly,
    validation,
    formStep
  );

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
    fieldSchema = value?.fieldSchema;
  });

  $: value = fieldState?.value ? fieldState.value : defaultValue;

  $: cellOptions = {
    placeholder,
    defaultValue,
    disabled,
    template,
    padding: "0.5rem",
    readonly: readonly || disabled,
    icon,
    error: fieldState.error,
    role,
    showDirty,
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      "flex-direction": labelPos == "left" ? "row" : "column",
      "align-items": "stretch",
      gap: labelPos == "left" ? "0.5rem" : "0rem",
      "grid-column": span < 7 ? "span " + span : "span " + groupColumns * 6,
      "--label-width":
        labelPos == "left" ? (labelWidth ? labelWidth : "6rem") : "auto",
    },
  };

  onDestroy(() => {
    fieldApi?.deregister();
    unsubscribe?.();
  });
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<div class="superField" use:styleable={$component.styles}>
  {#if label && labelPos}
    <label for="superCell" class="superlabel" class:left={labelPos == "left"}>
      {label}
      {#if fieldState.error}
        <div class="error" class:left={labelPos == "left"}>
          <span>{fieldState.error}</span>
        </div>
      {/if}
    </label>
  {/if}

  <div class="inline-cells">
    <CellDatetime
      bind:cellState
      {cellOptions}
      value={fieldState.value}
      {fieldSchema}
      {autofocus}
      on:change={(e) => fieldApi?.setValue(e.detail)}
      on:blur={cellState.lostFocus}
    />

    {#if customButtons && buttons?.length}
      <div
        class="spectrum-ActionGroup spectrum-ActionGroup--compact spectrum-ActionGroup--sizeM"
        class:spectrum-ActionGroup--quiet={buttonsQuiet}
      >
        {#each buttons as { text, onClick, quiet, disabled, type }}
          <SuperButton
            quiet={buttonsQuiet || quiet}
            {disabled}
            size="M"
            {text}
            onClick={enrichButtonActions(onClick, $allContext)}
            emphasized
            selected={type == "cta"}
          />
        {/each}
      </div>
    {/if}
  </div>
</div>
