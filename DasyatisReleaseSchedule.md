---
title: DasyatisReleaseSchedule
import:
  - templates/releasescheduletable

events:
  - done: yes
    component: systemd
    description: Fix Debian bug#738965 in Tanglu
        (and ideally Debian as well) by providing systemd units
    contact: Nobody (@null)

  - inprogress: yes
    component: debian-installer
    description: Provide a working debian installer
    contact: Philip Muskovac (@yofel)
---

Dasyatis kuhlii Release Schedule
--------------------------------

Please note that we can't really do a perfect release schedule - so many things are new and we have few contributors. Some events on this table have been adjusted (and will be adjusted), some have been reconstructed from the past to have a complete schedule.

{{> templates/releasescheduletable schedule_events=events}}

<table>
<colgroup>
<col width="15%" />
<col width="28%" />
<col width="28%" />
<col width="28%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><p>Date</p></th>
<th align="left"><p>Dasyatis Event</p></th>
<th align="left"><p>Art tasks</p></th>
<th align="left"><p>PR tasks</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>04. August 2015</p></td>
<td align="left"><p>Dasyatis branched off from Chromodoris</p></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>06. August 2015</p></td>
<td align="left"><p>Automatic syncs from Debian Testing enabled.</p></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>26. October 2015</p></td>
<td align="left"><p>Alpha Snapshot release</p></td>
<td align="left"></td>
<td align="left"><p>Announce on blog, social media &amp; mailing list. Send out press release.</p></td>
</tr>
<tr class="even">
<td align="left"><p>28. December 2015</p></td>
<td align="left"><p>Debian Import Freeze</p></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>24. January 2016</p></td>
<td align="left"><p>Initial QA round completed.</p></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>26. January 2016</p></td>
<td align="left"><p>Feature freeze.</p></td>
<td align="left"><p>Prepare installer slide show images</p></td>
<td align="left"><p>Announce on social media (C) &amp; mailing list.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>02. February 2016</p></td>
<td align="left"><p>Beta images built</p></td>
<td align="left"></td>
<td align="left"><p>Announce on blog, social media &amp; mailing list. Send out press release. Ask for testing on social media (C).</p></td>
</tr>
<tr class="even">
<td align="left"><p>20. February 2016</p></td>
<td align="left"><p>Installer testing completed</p></td>
<td align="left"></td>
<td align="left"><p>Announce on social media (C) and ask for testing.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>22. February 2016</p></td>
<td align="left"><p>Final QA round completed</p></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>24. February 2016</p></td>
<td align="left"><p>Release candidate images built. Final freeze, write release notes!</p></td>
<td align="left"><p>Prepare banners for social media</p></td>
<td align="left"><p>Announce on blog, social media &amp; mailing list. Send out press release.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>28. February 2016</p></td>
<td align="left"><p>Tanglu 4.0 (Dasyatis kuhlii) released!</p></td>
<td align="left"><p>Change banner graphics on social media</p></td>
<td align="left"><p>Announce on blog, social media &amp; mailing list. Send out press release.</p></td>
</tr>
</tbody>
</table>

*Notes:*
Social media posts marked with (C) are community-only updates for G+ community and forum. If you want to take over PR tasks please head over to the [bug tracker](https://tracker.tanglu.org/maniphest/query/PVBONqqPxA_0/#R) or contact [vinz](/user:vinzv "wikilink"). Also if you miss something here.

[category:Tanglu PR](/category:Tanglu_PR "wikilink")[category:Tanglu Art](/category:Tanglu_Art "wikilink")[category:ReleaseSchedule](/category:ReleaseSchedule "wikilink")[category:Dasyatis](/category:Dasyatis "wikilink")