EU VAT rates database
=====================


Starting January 1st, 2015, all european businesses are required to apply the
VAT depending on the locality of their customer.

This project aims to centralize, in a machine-readable format, the list of
applicable rates for each country and all their territorial exceptions.


Schema
------

`start_date` is an inclusive [ISO 8601 calendar date]
(https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) from which the rate
starts to apply.

`stop_date` is an inclusive [ISO 8601 calendar date]
(https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) from which the rate is
no longer valid.

`territory_codes` is a list of
  * [ISO 3166-1 alpha-2 country codes]
  (https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2),
  * [European Commission country codes]
  (http://publications.europa.eu/code/pdf/370000en.htm#pays),
  * [ISO 3166-2 subdivision codes](https://en.wikipedia.org/wiki/ISO_3166-2),
  * [normalized postal code]
  (https://en.wikipedia.org/wiki/Postal_code#Country_code_prefixes) with a
  leading ISO 3166-1 alpha-2 country codes.

`standard_rate` is the decimal standard VAT rate.

`description` human readable description of the territory the rate applies to.


Sources
-------

* http://ec.europa.eu/taxation_customs/taxation/vat/how_vat_works/index_en.htm
* http://ec.europa.eu/taxation_customs/resources/documents/taxation/vat/how_vat_works/rates/vat_rates_en.pdf
* http://ec.europa.eu/taxation_customs/taxation/vat/how_vat_works/telecom/index_en.htm
* https://en.wikipedia.org/wiki/European_Union_Value_Added_Tax_Area
* https://en.wikipedia.org/wiki/Special_member_state_territories_and_the_European_Union


License
-------

The content of this repository is licensed under a [BSD 2-Clause
License](./LICENSE.md).
