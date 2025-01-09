<script>
  import { getContext, onDestroy } from "svelte";
  import CellDatetime from "../../bb_super_components_shared/src/lib/SuperTableCells/CellDatetime.svelte";
  import SuperButton from "../../bb_super_components_shared/src/lib/SuperButton/SuperButton.svelte";
  import SuperFieldLabel from "../../bb_super_components_shared/src/lib/SuperFieldLabel/SuperFieldLabel.svelte";
  import "../../bb_super_components_shared/src/lib/SuperFieldsCommon.css";
  import "../../bb_super_components_shared/src/lib/SuperTableCells/CellCommon.css";

  const { styleable, enrichButtonActions, Provider, processStringSync } =
    getContext("sdk");
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
  export let controlType;

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

  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: labelPos = label
    ? groupLabelPosition && labelPosition == "fieldGroup"
      ? groupLabelPosition
      : labelPosition
    : false;

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

  $: cellOptions = {
    placeholder,
    defaultValue,
    padding: "0.5rem",
    disabled: disabled || groupDisabled || fieldState?.disabled,
    template,
    readonly: readonly || fieldState?.readonly,
    icon,
    error: fieldState?.error,
    role,
    showDirty,
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      "grid-column": span < 7 ? "span " + span : "span " + groupColumns * 6,
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
  <div
    class="superField"
    class:left-label={labelPos == "left"}
    class:multirow={controlType != "select"}
  >
    <SuperFieldLabel
      {labelPos}
      {labelWidth}
      {label}
      {helpText}
      error={fieldState?.error}
    />

    <div class="inline-cells">
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
    </div>
  </div>
</div>
