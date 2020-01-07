# 💸 [Digital,Cloud,Electronic,Online] Services VAT Rate Database

Some countries requires businesses to apply, for any sales of online/electronic/digital/cloud services to
consumers (B2C), a [value-added tax 
(VAT)](https://en.wikipedia.org/wiki/Value-added_tax) on all purchases made by their citizen. This impose all foreign companies to track the
residency of all their customers, to apply the right tax.

This trend started January 1st 2015 with all European country, and is about to be
implemented by others too.

As the rate depends on the locality of the customer, this project aims to centralize,
in a machine-readable format (currently a
plain CSV file), the list of applicable rates for each country of
residence, and all their territorial exceptions.

This is a painful job, worth sharing with a community, so please help me keep this database up to date! :)


## Testimonials

> I'm impressed with your BFPO detail.
-- [Tim Whitlock](https://twitter.com/timwhitlock/status/652464484578144256)

> I'm glad to see independently researched data confirming mine!
-- [Will Bond](https://twitter.com/wbond/status/560532109304291331)


## VAT Application Rules

All B2C customers matching the locality in that file are subject to the corresponding tax.

Your B2B customers are exempted of VAT, as long as they provide a
registered VAT number. You can check their validity on the [VAT Information
Exchange System (VIES)](https://ec.europa.eu/taxation_customs/vies/). I
recommend using a third-party library to automate the process, like 
[pyvat](https://github.com/iconfinder/pyvat) for Python. A B2B customer without VAT
number is considered as a simple B2C customer, so local rate applies.

Note that starting January 1st, 2015, these [rules applies to all non-European SaaS
businesses](https://ec.europa.eu/taxation_customs/taxation/vat/how_vat_works/telecom/index_en.htm#new_rules)
with European customers.


## Status

This matrix expose the current completeness of the database:

Administrative family | [EU member states](https://en.wikipedia.org/wiki/Member_state_of_the_European_Union) | [Special territories](https://en.wikipedia.org/wiki/Special_member_state_territories_and_the_European_Union), states, countries, collectivities, islands, departments, towns, …
:--- |:--- |:---
Number | :white_check_mark: 28 / 28 | :white_check_mark: 57 / (?)
Standard rates | :white_check_mark: All | :white_check_mark: All
Reduced rates | :x: None | :x: None
Increased rates | :x: None | :x: None
Parking rates | :x: None | :x: None
Currency codes | :white_check_mark: All | :white_check_mark: All
Historical standard rates | :white_check_mark: All | :x: None
Historical reduced rates | :x: None | :x: None
Historical increased rates | :x: None | :x: None
Historical parking rates | :x: None | :x: None
Historical currency codes | :warning: Wrongly aligned to current one | :warning: Wrongly aligned to current one


## Schema

`start_date` is an inclusive [ISO 8601 calendar 
date](https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) from which the rate
starts to apply.

`stop_date` is an inclusive [ISO 8601 calendar 
date](https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) from which the rate is
no longer valid.

`territory_codes` is a list of (eventually mixed):
  * [ISO 3166-1 alpha-2 country 
  codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2),
  * [European Commission country 
  codes](http://publications.europa.eu/code/pdf/370000en.htm#pays),
  * [ISO 3166-2 subdivision codes](https://en.wikipedia.org/wiki/ISO_3166-2),
  * [normalized postal 
  code](https://en.wikipedia.org/wiki/Postal_code#Country_code_prefixes) with a
  leading ISO 3166-1 alpha-2 country codes.

`currency_code` is the [*de jure*](https://en.wikipedia.org/wiki/De_jure)
[ISO 4217 currency code](https://en.wikipedia.org/wiki/ISO_4217) (a.k.a.
legal tender), not *de facto*'s one.

`rate` is the decimal rate.

`rate_type` is the kind of rate. Either:
  * `standard`
  * `increased`
  * `reduced`
  * `parking`

`description` human-readable description of the territory the rate applies to,
and eventual rationale behind the application.

Rows are sorted by `territory_codes`, then `start_date`.


## Interpretation

Starting from this database, your next step is to interpret the data.

By looking at the dates, you can compute if a rate is either current,
historical or future. Beware, some rates changes in the middle of a month.
That means on theory, your billing system should support pro-rata application
of several rates on a monthly invoice.

To choose the right rate, you then need to guess the location of your customer.
I advise you to derive this data from the billing address, as it's the most
common element with the necessary administrative granularity. An address that
is properly normalized is precise enough, down to the postal code, to select
the right VAT rule, including territorial exceptions. To solve the territory
complex, I wrote a [Python module to parse and normalize postal 
addresses](https://github.com/online-labs/postal-address).


## Sources

The process of building up this database is somewhat fuzzy.

This database is unequivocally founded on the latest [official VAT 
Rates](https://ec.europa.eu/taxation_customs/resources/documents/taxation/vat/how_vat_works/rates/vat_rates_en.pdf)
document from the EC portal. It provides all member states' rates and their
historical values. You'll also find there a description of regions and
territories where special or no VAT rates applies.

Still, the hardest part of establishing this database lies in the
characterization of locality. Member states and some regions are easy: they
have a dedicated country code. For these we rely on [ISO 3166-1 
alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2), with an extra compatibility
layer for [European Commission country 
codes](http://publications.europa.eu/code/pdf/370000en.htm#pays) (i.e. the `GB`/`UK`
and `GR`/`EL` pairs).

When this is not enough, we go down to a lower administrative level and
leverage subdivision codes from [ISO 
3166-2](https://en.wikipedia.org/wiki/ISO_3166-2).

Things get messy once VAT rules only applies to areas as small as a town. In
which case I guesstimated the geographic zone with postal codes fetched from
individual Wikipedia pages.

Finally, for completeness, I compiled the catalog of [member's states special
territories](https://en.wikipedia.org/wiki/Special_member_state_territories_and_the_European_Union#Summary)
and restarted the locality characterization process for these. I was able to
add the missing entries based on the list of included and excluded zones of the
[EU VAT area](https://en.wikipedia.org/wiki/European_Union_Value_Added_Tax_Area#EU_VAT_area).


## Other resources

* [official 
documentation](https://ec.europa.eu/taxation_customs/taxation/vat/how_vat_works/index_en.htm)
* [Rachel Andrew's micro-site](https://rachelandrew.github.io/eu-vat/)
* [Awesome Billing and Payments](https://github.com/kdeldycke/awesome-billing) - 💰 Billing & Payments Knowledge Base for Cloud Providers.


## History

I decided to create this database because all the [current VAT libs were quite
naive](https://github.com/kdeldycke/vat-rates/issues/2#issuecomment-67084124)
about the territory definition. Most of the time it's only based on the
country, while the territory a tax applies to, in a fiscal context, is a much
more insidious concept carrying administrative, political and historical
weight.

To match the place the supply takes place against the VAT database, I created a
[Python module to normalize and parse postal 
addressed](https://github.com/online-labs/postal-address) of my customers.


## License

The content of this repository is licensed under a [BSD 2-Clause 
License](./LICENSE.md).
