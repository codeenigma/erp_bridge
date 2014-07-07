Written by Salvador Molina (salvador.molinamoreno@codeenigma.com).
https://github.com/codeenigma/erp_bridge/

Provides a simple interface to prepopulate Form API fields with entity reference
values taken from the URL.

It's based on the entityreference_prepopulate module, and uses it to fetch the
values and check access permission when prepopulating the values. The reason to
write this custom module, is because entityreference_prepopulate is written to
work with 'entityreference' fields.

However, getting it to work with custom Form API fields that are used to reference
entities (through the usual entity label and entity id values used by entityreference),
it's a desirable feature, hence this bridge module.

Usage example:

When building a form, each custom field that is used to reference to an entity,
should declare some properties for the erp_bridge to work properly. The settings
have to be declared in a form property called '#erp_bridge_fields'. So,
if you have 2 fields for entityreferences, called "prepopulate_field_1" and
 "prepopulate_field_2", the "erp_bridge_fields" would look like this:

$form['#erp_bridge_fields'] = array(
  'prepopulate_field_1' => array(...),
  'prepopulate_field_2' => array(...),
);

For example, if the form has a field called "project_id", used to reference
"project" entities, and it's used to build a "sprint" entity,
the settings array would look like this:

  $form['#erp_bridge_fields'] = array(
    'project_id' => array(
      'target_type' => 'project',
      'entity_type' => 'sprint',
      'bundle' => 'sprint',
      'target_bundles' => array(),
      'prepopulate_settings' => array(),
    ),
  );

Note that the index of each settings array, is the same as the actual field name.

Explanation of properties:

    'target_type' => The entity type being referenced,
    'entity_type' => The entity type from which the reference is made,
    'bundle'      => Bundle from which the reference is made,
    'target_bundles' => Array of target bundles (empty array for all of them),
    'prepopulate_settings' => Array with any desired prepopulate settings,

After the #erp_bridge_fields has been filled, the next function needs to be
called before the $form array is returned to the render function:

  erp_bridge_prepopulate_form($form, $form_state);

For an explanation of the 'prepopulate_settings' array, see the README of the
entityreference_prepopulate module. In most cases an empty array can be passed
here and defaults will be ok to use.