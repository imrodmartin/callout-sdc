Callout — a Single Directory Component

Companion files for **"Build a Single Directory Component From Scratch, Then Drop It
Into Drupal Canvas (Full Walkthrough)"** on drupalhelps.com.

A boxed note with three tones and a slot for arbitrary content. Small on purpose —
it is meant to be typed in five minutes and then reused.

Install

Copy the callout directory into your theme's components/ directory:

themes/custom/MY-THEME/
└── components/
    └── callout/
        ├── callout.component.yml
        ├── callout.twig
        └── callout.css

Then rebuild caches:

drush cr

Change group: MY-THEME in callout.component.yml to your theme name so the
component groups sensibly in the Canvas component list. Components with no group
key land in "Other".

Use it from a template

{% set callout_body %}
  <p>Back up your database before running updates.</p>
{% endset %}

{{ include('MY-THEME:callout', {
  tone: 'warning',
  heading: 'Before you upgrade',
  content: callout_body,
}, with_context = false) }}

Replace MY-THEME with your theme's machine name in the namespace too.

The with_context = false is deliberate, not a style preference. Canvas passes
props in directly, so a component that quietly depends on ambient render context
will work in Twig and then misbehave in Canvas. Testing with context off is how
you find that early.

Files

File	Required	Purpose
callout.component.yml	yes	Metadata, props schema, slots. Without it Canvas has no idea what props or slots exist.
callout.twig	yes	Markup. Note the extension: .twig, not .html.twig.
callout.css	no	Loads automatically because it is named after the component. No library definition, no attach.
	Also supported but not included here: callout.js, README.md, thumbnail.png,
and an assets/ directory.

Notes for Canvas

examples are doing real work. type, title, enum, examples,
  description and required all feed the Canvas prop form, and examples act as
  default values and inform the component preview. Strip them and editors get an
  empty form and a blank preview.
tone is an enum prop, not a variant. Component variants were added in
  Drupal core 11.2.0 but are not yet supported in Canvas, so an enum prop is the
  option that works today.
status: stable keeps the component visible. Canvas shows components marked
  stable, experimental or deprecated; obsolete is hidden. A component with no
  status key is available.
Prop types are uneven in Canvas. string, number, integer and boolean
  behave. Array support is incomplete and object needs Canvas $ref schemas.
  This component sticks to strings for that reason.

Validating

Schemas are optional for themes and mandatory for modules, but validate anyway:

composer require --dev drupal/core-dev
drush en sdc_devel -y
drush sdcv

To make schema compliance non-optional across your theme, add
enforce_prop_schemas: true to MY-THEME.info.yml. Data validation also needs
assertions enabled — ini_set('zend.assertions', 1); in settings.php if your
environment has them off.

Sources

Full reference list is in the post. The two that matter most here are the official
docs on creating a single-directory component
and Michael Anello's Florida DrupalCamp session, *Creating Single Directory
Components with Drupal Canvas in mind*.
