---
title: ADAPT Storyboard Field and Evidence Schema
schema_id: ADAPT-PPA-FIELDS
version: "1.0"
purpose: Defines fields the review agent may encounter and how missing values must be interpreted.

controlled_value_status:
  - true
  - false
  - unknown
  - not_applicable
  - not_extracted

missing_data_policy:
  - Missing, unknown, and not_extracted are not equivalent to false.
  - not_applicable may be used only when the field cannot apply to that object/component type.
  - If a rule requires an unavailable field, classify the rule as REVIEW or POTENTIAL_FAILURE as specified.

common_fields:
  identity: [object_id, object_type, parent_id, page_id]
  naming: [title, display_title, aria_label, title_hidden]
  visibility: [hidden, visible_to_learner]
  navigation_exposure: [search_indexed, page_level_progress_enabled, main_menu_exposed, screen_reader_region_exposed]
  completion: [required_for_completion, optional, completion_criteria, completion_status]
  ordering: [sequence_number, position_on_page]

objects:
  Page:
    expected_fields: [page_id, title, display_title, title_hidden, page_position, search_enabled, incomplete_prompt_enabled]
  Article:
    expected_fields: [article_id, title, display_title, title_hidden, hidden, trickle_enabled, trickle_level]
  Block:
    expected_fields: [block_id, title, display_title, title_hidden, hidden, trickle_enabled, trickle_level]
  Component:
    expected_fields: [component_id, component_type, title, display_title, title_hidden, hidden, instructions, required_for_completion, optional, completion_criteria, page_level_progress_enabled, search_indexed, item_count, aria_label]
  Question:
    expected_fields: [component_id, component_type, title, instructions, required_for_completion, submit_required, attempts, completion_criteria]
  Quick_Navigation:
    expected_fields: [component_id, title, instructions, enabled_controls, visible_labels, aria_labels, sequence_number]

field_meanings:
  title: Internal or exposed title as provided by the storyboard.
  display_title: Learner-facing title when separately provided.
  title_hidden: Whether the title is hidden from its normal learner-facing display. This does not by itself prove exclusion from Search, PLP, menus, or screen-reader structure.
  hidden: Whether the entire object is unavailable to the learner.
  visible_to_learner: Whether the object/content is presented to the learner.
  search_indexed: Whether the object appears as a Search result/destination.
  page_level_progress_enabled: Whether the object is represented in Page Level Progress.
  required_for_completion: Whether failure to complete the object can prevent page completion/progression.
  optional: Whether interaction is not required for completion.
  completion_criteria: The event(s) needed for the object to register complete.
  item_count: Number of panels, pins, tiles, slides, steps, options, or other repeated items.
  page_position: first_page, middle_page, last_page, standalone, or unknown.
  enabled_controls: Quick Navigation controls enabled for the page.
  runtime_keyboard_test: observed pass/fail result from Preview; normally absent from a storyboard.
  runtime_screen_reader_test: observed pass/fail result from Preview; normally absent from a storyboard.

interpretation_rules:
  - Prefer display_title when it is explicitly the learner-facing label; otherwise use title and state the limitation.
  - title_hidden does not prove search_indexed=false or page_level_progress_enabled=false.
  - optional=true normally implies required_for_completion=false; if both are true, report contradictory evidence for REVIEW.
  - Instructions are evidence of what the learner is told, not proof of configuration.
  - Component type is not proof that the component is required.
  - A visible title is not proof that it is indexed by Search or PLP.
