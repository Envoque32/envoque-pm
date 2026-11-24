# Ènvoque Property Management landing page

One page that captures Property Management enquiries from the three PM campaigns and files them for handover.

Live address: https://envoque32.github.io/envoque-pm/

## One page, three campaigns

The campaign is chosen by the link. The page swaps its headline, supporting copy, thank-you message and the one tailored question.

| Link | Campaign |
|---|---|
| `/?c=dissatisfied_landlord` | Dissatisfied Landlord (asks when they would consider a change) |
| `/?c=self_managing_landlord` | Self-Managing Landlord (asks what takes the most time) |
| `/?c=bring_a_mate` | Bring a Mate (opens the mate fields and permission box) |
| `/` | Neutral page, no campaign tag |

Add `&utm_source=facebook&utm_campaign=...&utm_content=...` to ad links. All three are stored on the lead.

Add `&test=1` to mark a submission as a drill. The lead is stored with a ZZ-LyraDrill name so the handover engine can never send it.

## What it captures

Step one (enough to get a call): name, mobile, email, suburb, number of investment properties.

Step two (helps the right person prepare): who manages it now, when they would consider a change or what takes the most time, whether they are bringing a mate (plus the mate's name, number and a permission tick), whether they are already an Ènvoque landlord, weekly rent band, current fee band, what frustrates them, best time to call, a free text note, and the consent tick.

## Where it goes

The form posts to the `submitPMIntake` backend function, which validates, requires consent, drops honeypot spam, rejects non-Australian numbers, de-duplicates on email or mobile for 24 hours, and writes a `PMLead` record (`source_channel: web_form`, `handover_status: pending`) with the consent wording and timestamp stored in `answers`.

It sends no email and contacts nobody. The one-thread handover to Nicholas Chant and Chloe is `pmLeadHandover`, which stays in dry-run until Troy approves and enables the send switch. Marketing sends nothing to the enquirer. Fiona is excluded.

Free text that mentions a sensitive situation (estate, divorce, hardship, legal action and similar) is flagged SENSITIVE so a human reads it before anyone calls.

## Before the Bring a Mate offer goes live

`T_AND_C_URL` in `index.html` is empty on purpose. Nicholas Chant must sign the Bring a Mate terms and conditions first. Once there is a link, set it and the footer shows it. Do not publish the offer in ads until then.

## Files

- `index.html` the whole page and its script
- `assets/` fonts (IvyPresto, Myriad, Panchang), the PM logos and web-sized portraits from the Marketing drive People and Faces folder

## Notes

- Portraits are Unsplash images. The licence allows commercial use but does not verify model releases, so identifiable faces carry a small residual risk on paid ads. Troy to sign off.
- Marked `noindex` so it is reached from ads and links, not search.
