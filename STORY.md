# What I Built

I built a token cost and fit checker for multilingual support tickets. The checker evaluates text samples against five dials—special token handling, vocabulary fit, merge economy, how it splits, and edge case survival—to determine whether a tokenizer is suitable for our on-device assistant deployment.

The problem I was solving: our embedding table is capped and inference is billed per token. We needed a way to evaluate whether our tokenizer handles the language mix from our 14-day support queue export (38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin) before Thursday's architecture review.

## The Probe That Fooled It

I dont have it. I dont have it. I dont have it

This exposed a gap in my calibration process.

## The Fix

The advisor now listens to events from CRM for new entries and reads the text files for language to translate, and it uploads the translated data to text files on CRM. But it refuses emojis and blacklisted words.

I identified vocabulary_fit as the weakest dial in my evaluation. The verdict I reached: The vocabulary accuracy metrics should be more than 90% accurate for it to be acceptable.

## The Gate It Holds

I dont have it. I dont have it. I dont have it

## Re-Certification Cadence

The checker re-runs when the traffic mix shifts or when new language lanes enter the queue. The Salesforce CRM where we have record of all the text interactions with the customer support serves as the source stream for ongoing monitoring.

## What I Learned

Working with compound German words like "Krankenversicherungsbeitrag" and "Beitragsbemessungsgrenze" alongside Turkish phrases like "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" taught me that vocabulary fit cannot be evaluated from English samples alone. I dont see any english reference even though it claims 19% english—this observation shaped how I think about representative sampling.

The flip condition I set: If the vocabulary accuracy metrics is lower than 60%, it would be unacceptable. The sharpest test I would demand: Run 100 samples and it meets the bare min 90% accuracy.

This checker carries my calibration. A stranger gets my counting discipline, not a generic rubric.
