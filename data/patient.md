// NZHealth -> NZ_FHIR -> Existing Consensis -> DHB's current mapping -> Consensis.


input
{
  id: NHI,
  timestamp: when the data happened, rather than reported if possable.
  dhb_shortcode: from the dim_DHB table, if not supplied, it will work it out from hospital short code because mapper.
  enthnicity?:from dim_ethnicity, but likely to be contentious AND should be part of NHI? - we should grab from NZ_FHIR...
  hospital_code: from dim_hospital.
  room_type: dim_room_type?
  specialty_code:
  ward: is there a universal ward code? or is it local per hospital (not a problem if it is..)
  ICU: true/false
  HDU: true/false
}

post map
{
  hospital_name: from hospital_code,
  dhb_short_code: from hospital_code,
  dhb_name: from dhb_shortcode,
  specialty: from specality_code,
  ward_type: MAYBE from hospital_code & ward? but may be too chaotic
  ICU: from ward where available. true/false
  HDU: from ward where available. true/false
}

So, do we store code or description or both or *automap code to description?*

specialty? is this a field with a known mapping?

Note to blair: multipass mapping. hospital -> dhb-code -> dhb_name, so map until record stops changing (set limit incase they fuck it up(20), log it)

discharge 

{id, timestamp, dhb_shortcode:null, hospital_code:null, room_type:null, specialty_code:null, ward: null, ICU:null, HDU:null}

{id, timestamp, discharged:true}




Ok, you were talking about Translation tables on github.

Who should be our contact for that?
codes for the DHBs
codes for hospitals.
codes for specialty? (maybe)
hosptial wards? is there a table specifying them across the country?
