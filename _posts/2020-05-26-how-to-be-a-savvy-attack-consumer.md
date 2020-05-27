---
layout:     post
title:      "How to Be a Savvy ATT&CK Consumer?"
date:       2019-07-01 12:00:00
author:     "Katie Nickels"
categories: "ATT&CK"
tags:
  - ATT&CK
  - MITRE
  - CTI
  - INTEL
---

<p>
	Over the past several years, it’s been exciting for us to see how MITRE ATT&CK has helped people across the community move toward a threat-informed defense. However, as ATT&CK has increased in popularity there’s a risk we all need to watch out for: people assuming ATT&CK is some kind of silver bullet that will solve all their problems. It isn’t magic and it does take work, but it can be a compelling way to improve your defenses with the right understanding of what it is. To that end, we want to share our thoughts on how you can be a savvy ATT&CK consumer as you decide how different approaches, vendors, products, and services can help you improve defenses. If someone says they’re using ATT&CK, what does that mean? We’ll focus on equipping you with questions to ask anyone who uses ATT&CK. If you’re an “end user” Security Operations Center, think about how you answer these questions for yourselves. If you’re considering security products, these are great questions to ask vendors. If you’re a vendor, we suggest preparing answers to these questions so your customers understand your value-added.
</p>

<h2 class="section-heading">Why are they using ATT&CK?</h2>

<p>
	There are a lot of great reasons to use ATT&CK. We’ve outlined four key use cases we’ve seen: detection and analytics, threat intelligence, assessment and engineering, and adversary emulation. To be sure it makes sense to use ATT&CK in the product or service you’re looking at, we recommend asking what problem they are trying to solve by using ATT&CK. Are they trying to ensure a breadth of detections on adversary behaviors? Are they trying to prioritize defenses based on what adversaries have been known to do? Are they trying to help consumers understand alerts in a common language? These are valid use cases, but for ATT&CK to be most effective, someone using ATT&CK should be able to explain what the reasons are they chose to use it.
</p>

<h2 class="section-heading">How are they mapping to ATT&CK?</h2>
<p>
	As someone applies unstructured data to any framework, there are two key approaches: making humans do the work and making machines do the work. Using ATT&CK is no different, and both manual and automated approaches can be valuable as someone is “mapping” to ATT&CK. (By mapping to ATT&CK, we mean the process of taking defensive artifacts like behavioral analytics or intel on adversary behaviors and assigning them to the applicable ATT&CK technique(s).) Anyone who is aligning to ATT&CK should be able to explain how they’re going about that. For example:
</p>

<ul>
	<li><p>Human analysts have reviewed ATT&CK technique descriptions and determined that an analytic addresses that technique. (Our team has done this in the Cyber Analytics Repository.)</p></li>
	<li><p>A script has run natural language processing (NLP) on procedure examples and then used machine learning approaches to find adversary behaviors in a large data set. (Our team has done this in the Threat Report ATT&CK Mapper (TRAM).)</p></li>
</ul>

<p>
	Manual and automated approaches each have strengths and weaknesses. While we can and should automate where we can, sometimes human judgment is needed for analysis. On the flipside, human analysis is prone to cognitive biases, as we’ve discussed before. To create the strongest analysis when mapping to ATT&CK, we recommend a hybrid approach combining the best that humans and machines have to offer. For example, with a technique like Rundll32 (T1085), it may be simple for a machine to map the executable rundll32.exe to that technique. However, for broader techniques like Masquerading, human analysis may be needed to determine if a malicious process is masquerading as a legitimate one like svchost.exe.
</p>

<h2 class="section-heading">What are the strengths and weaknesses to their approach?</h2>
<p>
	I often quote the saying from George Box: “all models are wrong, some models are useful.” ATT&CK isn’t exempt from this. While ATT&CK is a useful framework, it has limitations — for example, it intentionally focuses on what real adversaries have done to help defenders prioritize, but that means it doesn’t cover everything an adversary could possibly do. Since ATT&CK has limitations, that means any method of using ATT&CK also has limitations. Since all approaches to using ATT&CK have pros and cons, we recommend asking what the limitations are to the selected approach. For example, are detections of Discovery techniques “brittle” because they only focus on one method of execution, such as looking at PowerShell but not Windows Management Instrumentation? As we talked about with the above section on human versus automated mapping, is the approach slower because humans are doing manual analysis? If someone using any model, including ATT&CK, says there are no limitations or weaknesses, we would consider this a red flag because that is unrealistic.
</p>

<h2 class="section-heading">How are they updating ATT&CK mappings?</h2>
<p>
	We add new techniques and information to ATT&CK several times a year, and we have a big move to sub-techniques coming up in 2020. Anyone using ATT&CK should know this and have a plan for how they will update their mappings when new or altered techniques are published. Of course, you should be reasonable and realistic about how quickly you expect someone to do this, since it takes time to change products and services — especially if significant human analysis is needed. (As a side note, we’ve heard from you that hearing more from us on our plans will help you better solidify YOUR plans, so we’re working on this!)
</p>

<h2 class="section-heading">What level of detail are they mapping to ATT&CK?</h2>
<p>
	When we explain ATT&CK, we describe that there are three levels of granularity (soon to be four):
</p>

<ul>
	<li><p>Tactics: the adversary’s technical goals. (For example, Credential Access.)</p></li>
	<li><p>Techniques: how the adversary achieves those goals. (For example, Credential Dumping, T1003.)</p></li>
	<li><p>Sub-techniques (coming soon™): a more specific description of the adversary behavior used to achieve a goal. (For example, Local Security Authority (LSA) Secrets.)</p></li>
	<li><p>Procedures: the specific implementation the adversary uses for a technique or sub-technique. (For example, a group using PowerShell to scrape LSASS memory and dump credentials on a victim.)</p></li>
</ul>

<p>You should ask what level of detail someone is using when they map to ATT&CK and why they chose that level. Our opinion is that the more detail you can map to, the more useful the information will be. For example, if I just know a detection alert is Execution (a tactic), I likely don’t have enough information to know what action I should take. Similarly, even mapping to the technique level may not be enough detail in some cases, particularly for broad techniques like Scripting (T1064). By getting to procedure-level detail, you know exactly how the adversary implemented the technique, which lets you be best informed to take action. For example, if I know an adversary has used Visual Basic scripts in Office documents, I can look for that in my logs.</p>

<h2 class="section-heading">How can you get more context about ATT&CK mappings?</h2>
<p>
	In considering level of detail, a related question to ask is how you can get additional context on the ATT&CK mapping. Without knowing the logic or approach behind a particular ATT&CK mapping, it may be difficult for you to know how to action it. For example, if a detection is labeled as with Registry Run Key (T1060), can you pivot within a tool to identify the key added? On the threat intelligence side, if a report indicates an adversary used a Standard Application Layer Protocol (T1071) for Command and Control, can you find out what protocol that is? Is that available directly in the report, or do you have to request it from the originator? With ATT&CK mappings, like threat intelligence more broadly, it’s important to be able to understand the context behind them.
</p>

<h2 class="section-heading">How much of ATT&CK do they cover?</h2>
<p>
	It is unrealistic for any single defensive product or service to cover all of ATT&CK. This is important because we’ve heard from vendors that their customers expect 100% coverage. As the team who created ATT&CK, we want to be absolutely clear that you should not expect 100% coverage of all ATT&CK techniques. For example, certain techniques like Supply Chain Compromise (T1195) are difficult to identify and may require specialized sources that are generally not available in cybersecurity products and services that monitor enterprises.
</p>

<p>
	When asking about coverage of ATT&CK, keep in mind that it’s a complex question. For example, the number of analytics mapped to a technique may not be the best metric of how well its procedures are covered. A single analytic for a technique might be more effective at identifying adversaries than 10 analytics for a different technique. Simply because a technique has more detectors for it does not mean its coverage is “better”. For this reason, we recommend using confidence levels to express coverage of techniques, such as by using a scale of one to five. Thanks to Olaf Hartong for this idea, which you could visually express in an ATT&CK Navigator layer by using shades of a color, with a darker shade indicating more confidence.
</p>

<img src="{{ site.baseurl }}/img/attack_map_blue.png" class="img-responsive" alt="ATT&CK Confidence Levels">
<span class="caption text-muted">Example of Confidence Levels in ATT&CK Navigator Layer (h/t to Olaf Hartong)</span>

<p>
	You should also remember that each ATT&CK technique may have many procedures for how an adversary could implement it — and because adversaries are always changing, we can’t know what all those procedures are in advance. That makes discussing coverage of a technique tough, especially when some ways of detecting behavior rely on individual procedures and some may span multiple procedures or even an entire technique. We encourage you to look at threat intelligence on what procedures adversaries have used and ask about those you care about most. Anyone mapping to ATT&CK should be able to explain the procedures they cover. Similarly to how it’s unrealistic to expect coverage of 100% of ATT&CK techniques, it’s unrealistic to expect coverage of all procedures of a given technique, especially since we can’t know all of them.
</p>

<p>
	While 100% coverage is unrealistic, you should ask about what parts of ATT&CK they cover and don’t cover. As you may be able to guess by now, we also recommend asking why they cover certain techniques and procedures and not others, which gives you helpful information about their approach. Once you have an understanding of what a particular source covers, you can then seek to complement that source with other approaches to fill gaps, as we’ve discussed. For example, perhaps no sources you find can cover Supply Chain Compromise (T1195), so you may need to work with your risk management team to address this if it is a risk for your organization. A savvy consumer of ATT&CK will realize that no single product or service is a “silver bullet” so we need to blend approaches to improve our defense-in-depth.
</p>

<h2 class="section-heading">In Conclusion</h2>
<p>
	We hope this blog post has helped you identify questions you can ask of anyone who uses ATT&CK, whether it’s a vendor or your own SOC. We want to ensure ATT&CK remains a useful resource for improving defenses in this community, and to that end, we hope this post has educated you on important aspects of being a savvy ATT&CK consumer.
</p>
<p>
	©2019 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 19–01159–20.
</p>

<p>Post original: <a href="https://medium.com/mitre-attack/how-to-be-a-savvy-attack-consumer-63e45b8e94c9"><em>How to Be a Savvy ATT&CK Consumer?</em></a></p>