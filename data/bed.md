more post when it is running.

{
  id -> hospital_code + _ + hospital_bed_id
  map from hospital_code to DHB etc see patient table for mappings.
  ICU flag
  HDU flag
  ward code
  free: is bed free?
  exists: does bed exist?
  resourced: flags? boolean? nurse table?
}

mappings
{
  free maps from exists.
}

