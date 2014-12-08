EU VAT rates database
====================


Starting January 1st, 2015, all european businesses are required to apply the VAT depending
on the locality of their customer.


Schema
------

`start_date` is an inclusive UTC date-time from which the rate starts to apply.

`stop_date` is an inclusive UTC date-time from which the rate is no longer valid.

`currency_code` is an ISO 4217 currency code.

`territory_code` is a list of
  * ISO 3166-1 alpha-2 country codes,
  * ISO 3166-2 subdivision codes, or
  * normalized postal code with a leading ISO 3166-1 alpha-2 country codes.
  
`rate` is the decimal VAT rate.
  
`description` describe the rate.


Sources
-------

* http://ec.europa.eu/taxation_customs/taxation/vat/how_vat_works/index_en.htm
* http://ec.europa.eu/taxation_customs/resources/documents/taxation/vat/how_vat_works/rates/vat_rates_en.pdf
* http://ec.europa.eu/taxation_customs/taxation/vat/how_vat_works/telecom/index_en.htm
* https://en.wikipedia.org/wiki/Special_member_state_territories_and_the_European_Union

