EU VAT rates database
=====================

Any european businesses are required to apply a [value-added tax (VAT)]
(https://en.wikipedia.org/wiki/Value-added_tax) on all purchases made by each
of their european individual customers (B2C). Starting January 1st, 2015, the
rate depends on the locality of the customer.

This project aims to centralize, in a machine-readable format (currently a
plain CSV file), the list of applicable rates for each european country of
residence, and all their territorial exceptions.

This is a painful job, so please help me keep this database up to date.


VAT application rules
---------------------

The locality rule only concerns your european B2C customers.

Your european B2B customers are exempted of VAT, as long as they provide a
registered VAT number. You can check their validity on the [VAT Information
Exchange System (VIES)](http://ec.europa.eu/taxation_customs/vies/). I
recommend using a third-party library to automate the process, like [pyvat]
(https://github.com/iconfinder/pyvat) for Python.

Note that starting January 1st, 2015, these [rules applies to all non-european
SaaS businesses]
(http://ec.europa.eu/taxation_customs/taxation/vat/how_vat_works/telecom/index_en.htm#new_rules)
with european customers.

Check the [official documentation]
(http://ec.europa.eu/taxation_customs/taxation/vat/how_vat_works/index_en.htm)
for more details.


Status
------

This matrix expose the current completeness of the database:

Administrative family | [EU member states](https://en.wikipedia.org/wiki/Member_state_of_the_European_Union) | [Special territories](https://en.wikipedia.org/wiki/Special_member_state_territories_and_the_European_Union)
:--- |:--- |:---
Number | 28 | ?
Standard rates | All | All
Reduced rates | None | None
Increased rates | None | None
Parking rates | None | None
Historical standard rates | All | None
Historical reduced rates | None | None
Historical increased rates | None | None
Historical parking rates | None | None


Schema
------

`start_date` is an inclusive [ISO 8601 calendar date]
(https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) from which the rate
starts to apply.

`stop_date` is an inclusive [ISO 8601 calendar date]
(https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) from which the rate is
no longer valid.

`territory_codes` is a list of (eventually mixed):
  * [ISO 3166-1 alpha-2 country codes]
  (https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2),
  * [European Commission country codes]
  (http://publications.europa.eu/code/pdf/370000en.htm#pays),
  * [ISO 3166-2 subdivision codes](https://en.wikipedia.org/wiki/ISO_3166-2),
  * [normalized postal code]
  (https://en.wikipedia.org/wiki/Postal_code#Country_code_prefixes) with a
  leading ISO 3166-1 alpha-2 country codes.

`rate` is the decimal rate.

`rate_type` is the kind of rate. Either:
  * `standard`
  * `increased`
  * `reduced`
  * `parking`
  * `regional`

`description` human-readable description of the territory the rate applies to,
and eventual rationale behind the application.

Rows are sorted by `territory_codes`, then `start_date`.


Sources
-------

The process of building up this database is somewhat fuzzy.

This database is unequivocally founded on the latest [official VAT Rates]
(http://ec.europa.eu/taxation_customs/resources/documents/taxation/vat/how_vat_works/rates/vat_rates_en.pdf)
document from the EC portal. It provides all member states' rates and their
historical values. You'll also find there a description of regions and
territories were special or no VAT rates applies.

Still, the hardest part of establishing this database lies in the
characterization of locality. Member states and some regions are easy: they
have a dedicated country code. For these we rely on [ISO 3166-1 alpha-2]
(https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2), with an extra compatibility
layer for [European Commission country codes]
(http://publications.europa.eu/code/pdf/370000en.htm#pays) (i.e. the `GB`/`UK`
and `GR`/`EL` pairs).

When this is not enough, we go down to a lower administrative level and
leverage subdivision codes from [ISO 3166-2]
(https://en.wikipedia.org/wiki/ISO_3166-2).

Things get messy once VAT rules only applies to areas as small as a town. In
which case I guesstimated the geographic zone with postal codes fetched from
individual Wikipedia pages.

Finally, for completeness, I compiled the catalog of [member's states special
territories]
(https://en.wikipedia.org/wiki/Special_member_state_territories_and_the_European_Union#Summary)
and restarted the locality characterization process for these. I was able to
add the missing entries based on the list of included and excluded zones of the
[EU VAT area]
(https://en.wikipedia.org/wiki/European_Union_Value_Added_Tax_Area#EU_VAT_area).


License
-------

The content of this repository is licensed under a [BSD 2-ClauseLicense]
(./LICENSE.md).
