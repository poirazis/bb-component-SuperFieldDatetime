<script>
  import { getContext, onDestroy } from "svelte";
  import {
    SuperButton,
    SuperField,
    CellDatetime,
    CellDateRange,
  } from "@poirazis/supercomponents-shared";

  const {
    styleable,
    enrichButtonActions,
    Provider,
    processStringSync,
    builderStore,
  } = getContext("sdk");
  const component = getContext("component");
  const allContext = getContext("context");

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const groupLabelPosition = getContext("field-group");
  const labelWidth = getContext("field-group-label-width");
  const groupColumns = getContext("field-group-columns");
  const groupDisabled = getContext("field-group-disabled");
  const formApi = formContext?.formApi;

  export let field = "Date Field";
  export let helpText;
  export let role = "formInpur";

  export let buttons = [];

  export let label;
  export let span = 6;
  export let placeholder;
  export let defaultValue;
  export let disabled;
  export let readonly;
  export let validation;
  export let template;
  export let controlType = "datetime"; // datetime | range

  export let icon;
  export let labelPosition = "fieldGroup";
  export let showDirty;
  export let autofocus;
  export let showTime;
  export let dateFormat = "default";
  export let invisible = false;

  let formField;
  let formStep;
  let fieldState;
  let fieldApi;
  let fieldSchema;
  let value;

  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: labelPos =
    groupLabelPosition && labelPosition == "fieldGroup"
      ? groupLabelPosition
      : labelPosition;

  $: formField = formApi?.registerField(
    field,
    "datetime",
    defaultValue,
    disabled || groupDisabled,
    readonly || groupDisabled,
    validation,
    formStep
  );

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
    fieldSchema = value?.fieldSchema;
  });

  $: value = fieldState?.value ? fieldState.value : defaultValue;
  $: error = fieldState?.error;

  $: cellOptions = {
    placeholder: placeholder ?? field,
    defaultValue,
    padding: "0.5rem",
    disabled: disabled || groupDisabled || fieldState?.disabled,
    template,
    readonly: readonly || fieldState?.readonly,
    icon,
    error: fieldState?.error,
    role,
    showDirty,
    showTime,
    dateFormat,
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      display:
        invisible && !$builderStore.inBuilder
          ? "none"
          : $component.styles.normal.display,
      opacity: invisible && $builderStore.inBuilder ? 0.6 : 1,
      "grid-column": groupColumns ? `span ${span}` : "span 1",
    },
  };

  onDestroy(() => {
    fieldApi?.deregister();
    unsubscribe?.();
  });
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<div use:styleable={$component.styles}>
  <Provider
    data={{
      value,
      formattedValue: processStringSync(template ?? "", $allContext),
    }}
  />
  <SuperField {labelPos} {labelWidth} {field} {label} {error} {helpText}>
    {#if controlType != "range"}
      <CellDatetime
        {cellOptions}
        {value}
        {fieldSchema}
        {autofocus}
        on:change={(e) => {
          value = e.detail;
          fieldApi?.setValue(e.detail);
        }}
      />
    {:else}
      <CellDateRange
        {cellOptions}
        {value}
        {fieldSchema}
        {autofocus}
        on:change={(e) => {
          value = e.detail;
          fieldApi?.setValue(e.detail);
        }}
      />
    {/if}

    {#if buttons?.length}
      <div class="inline-buttons">
        {#each buttons as { text, onClick, quiet, disabled, type, size }}
          <SuperButton
            {quiet}
            {disabled}
            {size}
            {type}
            {text}
            on:click={enrichButtonActions(onClick, $allContext)({ value })}
          />
        {/each}
      </div>
    {/if}
  </SuperField>
</div>
