---
layout: default
title: "Ukraine’s IT Army and Hermetic Wiper"
date: 2022-04-01 15:00:00 -0000
---

---

# {{ page.title }}

**Published:** {{ page.date | date: "%B %d, %Y" }}

---


Back in the beginning of the conflict, as an individual whose opinion really doesn’t matter in the slightest, I was really appalled. Such senseless aggression over..what? A pipeline? Power? Land?

I got a link from a contact to join the “Ukraine IT Army” telegram. This was my first introduction to Telegram as an application..I’ve been partial to Signal.

Since joining, I promise I haven’t participated in any hacktivism..scout’s honor.

But, I have been receiving tons of OSINT, target lists, etc. and met a few wonderful and knowledgeable people.

Here are some of the Ukraine IT Army’s initial targets:

sberbank.ru

vsrf.ru

scrf.gov.ru

kremlin.ru

radiobelarus.by

rec.gov.by

sb.by

belarus.by

belta.by

tvr.by

I definitely did absolutely no passive intel scanning on any of these targets and found absolutely nothing

Part of the “ask” of the organizers of this group is simply “Users can scan for vulnerabilities instead of actively exploiting them and submit findings”. I thought this was pretty thoughtful for those unwilling to take the risk, or didn’t necessarily have the proper OpSec to undertake such an endeavor.

They collect these vulnerabilities under a compendium of Russian Infrastructure vulnerabilities. This is, of course, in response to several Russian attacks against Ukraine.

Hermetic Wiper

So, this appears to be a custom-written application with very few standard functions..sample I obtained was a measly 114KBs.

Essentially, this abuses partition management drivers in order to carry out more damaging/ruthless components of the attack (e.g. empntdrv.sys).

Hermetic Wiper uses a (relatively 😉) benign and legitimate EaseUS driver, typically used for data recovery. One way to detect this vulnerability is looking for write activity to this driver, as that is completely non-standard. Oops!

The driver is loaded by HW and executed to provide raw disk access to all mounted physical drives and is then granted access by this driver to access MBR of every drive, overwrite with RNG data and corrupt them beyond repair.

It also modifies several registry keys, including setting SYSTEM\CurrentControlSet\Control\CrashControl CrashDumpEnabled key to 0, effectively disabling crash dumps before the abused driver’s exec. This is an area of the registry I am intimately familiar with in terms of forensics.

Then…creepily, HW waits on sleeping threads before initiating a shutdown. That drive’s never spinning back up.

If you’d like the HW sample, please let me know!